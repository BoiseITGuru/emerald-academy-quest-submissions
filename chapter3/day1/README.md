## Chapter 3 - Day 1

1. In words, list 3 reasons why structs are different from resources.
    * The major differences are that resources cannot be copied, they cannot be created outside of the contract, and you must explictly move or destroy them.
2. Describe a situation where a resource might be better to use than a struct.
    * Aside from standard art NFTs as we know them today, one simple example would be a dapp for chain of custody. You would have a "resrouce" for a paticular item you a tracking, then each time that item changes custody you could have the end user call a function to create a new resrouce owned by the item resource with the details of exchange. The security of resources over structures and the permission systems within cadence/flow allows this to be a verifiable ledger system that could be trusted to be 100% accurate 
3. What is the keyword to make a new resource?
    * create
4. Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?
    * No they can only be created from within the contract to ensure their security.
5. If I am understanding the questions correctly would the type be @Jacob??
6. I Spy - Corrected Code
    * 
    ``` cadence
    pub contract Test {
        pub resource Jacob {
            pub let rocks: Bool
            init() {
                self.rocks = true
            }
        }

        pub fun createJacob(): @Jacob {
            let myJacob <- create Jacob()
            return <- myJacob
        }
    }
    ```