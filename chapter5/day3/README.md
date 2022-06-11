## Chapter 5 - Day 3

1. What does "force casting" with as! do? Why is it useful in our Collection?
    * Force casting allows us to downgrade from a more generic type to a more specefic type. This is useful so that we can actually store or load our NFT type and not that of the generic NFT type. This of course has wider ussage for all interface types.
2. What does auth do? When do we use it?
    * auth is how we use "force casting" for references to resources. We first create an auth reference to the generic type then force cast it using as!
3. This last quest will be your most difficult yet. [Take this contract](https://github.com/emerald-dao/beginner-cadence-course/tree/main/chapter5.0/day3#quests) and add a function called borrowAuthNFT just like we did in the section called "The Problem" above. Then, find a way to make it publically accessible to other people so they can read our NFT's metadata. Then, run a script to display the NFTs metadata for a certain id. You will have to write all the transactions to set up the accounts, mint the NFTs, and then the scripts to read the NFT's metadata. We have done most of this in the chapters up to this point, so you can look for help there :)
    1. Updated Contract
        ``` cadence
        import NonFungibleToken from 0x02
        pub contract CryptoPoops: NonFungibleToken {
            pub var totalSupply: UInt64

            pub event ContractInitialized()
            pub event Withdraw(id: UInt64, from: Address?)
            pub event Deposit(id: UInt64, to: Address?)

            pub resource NFT: NonFungibleToken.INFT {
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

            pub resource interface CollectionPublic {
                pub fun deposit(token: @NonFungibleToken.NFT)
                pub fun getIDs(): [UInt64]
                pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT
                pub fun borrowAuthNFT(id: UInt64): &NFT
            }

            pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, CollectionPublic {
                pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

                pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
                let nft <- self.ownedNFTs.remove(key: withdrawID) 
                        ?? panic("This NFT does not exist in this Collection.")
                emit Withdraw(id: nft.id, from: self.owner?.address)
                return <- nft
                }

                pub fun deposit(token: @NonFungibleToken.NFT) {
                let nft <- token as! @NFT
                emit Deposit(id: nft.id, to: self.owner?.address)
                self.ownedNFTs[nft.id] <-! nft
                }

                pub fun getIDs(): [UInt64] {
                return self.ownedNFTs.keys
                }

                pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
                return &self.ownedNFTs[id] as &NonFungibleToken.NFT
                }

                pub fun borrowAuthNFT(id: UInt64): &NFT {
                    let ref = &self.ownedNFTs[id] as auth &NonFungibleToken.NFT
                    return ref as! &NFT
                }

                init() {
                self.ownedNFTs <- {}
                }

                destroy() {
                destroy self.ownedNFTs
                }
            }

            pub fun createEmptyCollection(): @NonFungibleToken.Collection {
                return <- create Collection()
            }

            pub resource Minter {

                pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
                return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
                }

                pub fun createMinter(): @Minter {
                return <- create Minter()
                }

            }

            init() {
                self.totalSupply = 0
                emit ContractInitialized()
                self.account.save(<- create Minter(), to: /storage/Minter)
            }
        }
        ```
    2. Transaction to create collection for second account
        ``` cadence
        import CryptoPoops from 0x01

        transaction() {

            prepare(signer: AuthAccount) {
                let newCollection <- CryptoPoops.createEmptyCollection()
                signer.save(<- newCollection, to: /storage/MyCollection)

                signer.link<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>(/public/MyCollection, target: /storage/MyCollection)
            }
        }
        ```
    3. Transaction to Mint NFT and transfer to second account
        ``` cadence
        import CryptoPoops from 0x01

        transaction(recipient: Address) {

            prepare(signer: AuthAccount) {
                let minter = signer.borrow<&CryptoPoops.Minter>(from: /storage/Minter) ?? panic("This signer is not the one who deployed the contract.")

                let recipientsCollection = getAccount(recipient).getCapability(/public/MyCollection).borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>() ?? panic("the user does not have a collection")

                let nft <- minter.createNFT(name: "Object of Unkown Origin", favouriteFood: "Apples", luckyNumber: 7)

                recipientsCollection.deposit(token: <- nft)
            }
        }
        ```
    4. Script to get NFT IDs from second account
        ``` cadence
        import CryptoPoops from 0x01

        pub fun main(address: Address): [UInt64] {
            let recipientsCollection = getAccount(address).getCapability(/public/MyCollection).borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>() ?? panic("the user does not have a collection")

            return recipientsCollection.getIDs()
        }
        ```
    5. Script to read borrowed NFT Metadata from second account
        ``` cadence
        import CryptoPoops from 0x01

        pub fun main(address: Address): String {
            let recipientsCollection = getAccount(address).getCapability(/public/MyCollection).borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>() ?? panic("the user does not have a collection")

            let recipientsNFT = recipientsCollection.borrowAuthNFT(id: 3)
            return "NFT Says: My name is ".concat(recipientsNFT.name).concat(", my favourite food is ").concat(recipientsNFT.favouriteFood).concat(", and my lucky number is ").concat(recipientsNFT.luckyNumber.toString())
        }
        ```
        ![Chapter 5 Day 3 Question 3 - Answer](/images/C5D3Q3.png)