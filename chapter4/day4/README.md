## Chapter 4 - Day 4

1. Take our NFT contract so far and add comments to every single resource or function explaining what it's doing in your own words. Something like this:
    ``` cadence
    pub contract CryptoPoops {
    pub var totalSupply: UInt64

    // This NFT resource contains the and ID field and several meta data fields

    pub resource NFT {
        pub let id: UInt64

        pub let name: String
        pub let favouriteFood: String
        pub let luckyNumber: Int

        init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
        self.id = self.uuid

        self.name = _name
        self.favouriteFood = _favouriteFood
        self.luckyNumber = _luckyNumber
        }
    }

    // This resource interface defines the publicly accessible methods for our NFT Collection

    pub resource interface CollectionPublic {
        pub fun deposit(token: @NFT)
        pub fun getIDs(): [UInt64]
        pub fun borrowNFT(id: UInt64): &NFT
    }

    // This is our NFT Collection, it implements the public collection interface and stores our NFTs allowing for deposits, withdrawals and borrowing of the NFT.

    pub resource Collection: CollectionPublic {
        pub var ownedNFTs: @{UInt64: NFT}

        // This functions is used to deposit NFTs into the collection.

        pub fun deposit(token: @NFT) {
        self.ownedNFTs[token.id] <-! token
        }

        //This function is used to withdraw NFTs from the collection.

        pub fun withdraw(withdrawID: UInt64): @NFT {
        let nft <- self.ownedNFTs.remove(key: withdrawID) 
                ?? panic("This NFT does not exist in this Collection.")
        return <- nft
        }

        //Returns all NFTs IDs of owned NFTs.

        pub fun getIDs(): [UInt64] {
        return self.ownedNFTs.keys
        }

        //Used for borrowing the NFT and reading its metadata without removing it from the collection.

        pub fun borrowNFT(id: UInt64): &NFT {
        return &self.ownedNFTs[id] as &NFT
        }

        // called when the collection is created and created an empty dictionary of owned NFTs.

        init() {
        self.ownedNFTs <- {}
        }
        
        // explictly destorys the nested dictionary of resources when the collection is destroyed.

        destroy() {
        destroy self.ownedNFTs
        }
    }

    // Creates an empty collection and returns it to the transaction calling it.

    pub fun createEmptyCollection(): @Collection {
        return <- create Collection()
    }

    // this is Minter resource which is the only place with the code to create NFTs or addtional minters.

    pub resource Minter {
        // Creates the NFTs and returns it to the transaction calling the function.

        pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
        return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
        }

        // creates a new Minter resource allowing for someone else to mint NFTs.

        pub fun createMinter(): @Minter {
        return <- create Minter()
        }

    }

    // called when the contract is deployed setting the supply variable to 0 and saving a Minter resource to the account storage.

    init() {
        self.totalSupply = 0
        self.account.save(<- create Minter(), to: /storage/Minter)
    }
    }
    ```