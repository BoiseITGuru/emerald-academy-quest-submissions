## Chapter 3 - Day 2

1. Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. 
    *
    ``` cadence
    pub contract ChainOfCustody {

        pub var itemsInCustody: @{String: CustodyItem}

        pub resource CustodyItem {
            pub let itemName: String
            pub let itemID: String
            pub var chainOfCustody: @[CustodyExhange]

            init(_itemName: String, _itemID: String) {
                self.itemName = _itemName
                self.itemID = _itemID
                self.chainOfCustody <- []
            }

            destroy() {
                destroy self.chainOfCustody
            }

            pub fun addCustodyExchange(custodyExhange: @CustodyExhange) {
                self.chainOfCustody.append(<- custodyExhange)
            }

            pub fun removeCustodyExhange(index: Int): @CustodyExhange {
                return <- self.chainOfCustody.remove(at: index)
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
            self.itemsInCustody <- {}
        }

        pub fun addCustodyItem(custodyItem: @CustodyItem) {
            self.itemsInCustody[custodyItem.itemID] <-! custodyItem
        }

        pub fun removeCustodyItem(key: String): @CustodyItem {
            let custodyItem <- self.itemsInCustody.remove(key: key) ?? panic("Could not find item in custody!")
            return <- custodyItem
        }

    }
    ```