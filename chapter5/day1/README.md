## Chapter 5 - Day 1

1. Describe what an event is, and why it might be useful to a client.
    * Events are a way to send notifications to clients off the blockchain letting them know that something has been changed and they should update their state accordingly.
2. Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.
    * 
    ``` cadence
    pub contract ChainOfCustody {
        pub event CustodyItemMinted(id: UInt64)

        pub resource interface IPublicCustodyItem {
            pub let itemName: String
            pub fun getCurrentCustodian(): String
        }

        pub resource CustodyItem: IPublicCustodyItem {
            pub let itemName: String
            pub let itemID: UInt64
            pub var chainOfCustody: @[CustodyExhange]

            init(_itemName: String) {
                self.itemName = _itemName
                self.itemID = self.uuid
                self.chainOfCustody <- []
            }

            destroy() {
                destroy self.chainOfCustody
            }

            pub fun addCustodyExchange(_custodiansName: String, _dateOfExchange: String) {
                self.chainOfCustody.insert(at: 0, <- create CustodyExhange(_custodiansName: _custodiansName, _dateOfExchange: _dateOfExchange))
            }

            pub fun removeCustodyExhange(index: Int): @CustodyExhange {
                return <- self.chainOfCustody.remove(at: index)
            }

            pub fun getCurrentCustodian(): String {
                return self.chainOfCustody[0].custodiansName
            }
        }

        pub resource CustodyExhange {
            pub let exchangeID: String
            pub let custodiansName: String
            pub let dateOfExchange: String

            init(_exchangeID: String, _custodiansName: String, _dateOfExchange: String) {
                self.exchangeID = _exchangeID
                self.custodiansName = _custodiansName
                self.dateOfExchange = _dateOfExchange
            }
        }

        init() {
            
        }

        pub resource CustodyItemMinter {

            pub fun createCustodyItem(_itemName: String): @CustodyItem {
                let custodyItem <- create CustodyItem(_itemName: _itemName)
                emit CustodyItemMinted(id: custodyItem.itemID)
                return <- custodyItem
            }

            pub fun createMinter(): @CustodyItemMinter {
            return <- create CustodyItemMinter()
            }

        }
    }
    ```
3. Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.
    1. 
    ``` cadence
    pub fun getCurrentCustodian(): String {
        pre {
            self.chainOfCustody.length > 0: "There is no current chain of custody for this item"
        }
        post {
            result.length > 0: "Valid Name"
        }
        return self.chainOfCustody[0].custodiansName
    }
    ```
    2. 
    ``` cadence
    pub fun addCustodyExchange(_custodiansName: String, _dateOfExchange: String) {
        pre {
            _custodiansName.length > 0: "Invalid Name"
            _dateOfExchange.length > 0: "Not valid date"

              
        }
        self.chainOfCustody.insert(at: 0, <- create CustodyExhange(_custodiansName: _custodiansName, _dateOfExchange: _dateOfExchange))
    }
    ```
    3. 
    ``` cadence
    pub fun removeCustodyExhange(index: Int): @CustodyExhange {
        pre {
            self.chainOfCustody[index] != nil 
        }
        return <- self.chainOfCustody.remove(at: index)
    }
    ```
4. For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.
    1. numberOne
        * Yes the length of 'Jacob' is 5 and would pass the pre checks to log the variable.
    2. numberTwo
        * Yes the length of 'Jacob' is greater than or equal to 0 and would pass the pre checks, futher the concat function will added to the name string thus allowing the result to pass the post check and return the value.
    3. numberThree
        * No this does not have the log() function written only the return. Because the post condition is checking if the value of number is result + 1 it will always fail returning the value of number back to 0.