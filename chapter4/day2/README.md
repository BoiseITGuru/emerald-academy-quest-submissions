## Chapter 4 - Day 2

1. What does .link() do?
    * .link() creates a symbolic link between where the resource exists in your storage and either /public/ or /private/ allowing you to create capabilities for the resource to be accessible by other people.
2. In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.
    * By definine the variables and/or functions we want to allow others to have access to in the resrouce interface we can then use then to limit access to the resrouce while in the /public/ path.
3. Deploy a contract that contains a resource that implements a resource interface. Then, do the following:
    ``` cadence
    pub contract ChainOfCustody {

        pub resource interface IPublicCustodyItem {
            pub let itemName: String
            pub fun getCurrentCustodian(): String
        }

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
                self.chainOfCustody.insert(at: 0, <- custodyExhange)
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

        pub fun createCustodyItem(_itemName: String, _itemID: String): @CustodyItem {
            return <- create CustodyItem(_itemName: _itemName, _itemID: _itemID)
        }

    }
    ```
    1. In a transaction, save the resource to storage and link it to the public with the restrictive interface.
    ``` cadence
    import ChainOfCustody from 0x01

        transaction(_itemName: String, _itemID: String) {

        prepare(signer: AuthAccount) {
            let custodyItem <- ChainOfCustody.createCustodyItem(_itemName: _itemName, _itemID: _itemID)
            signer.save(<- custodyItem, to: /storage/MyCustodyItem)

            signer.link<&ChainOfCustody.CustodyItem{ChainOfCustody.IPublicCustodyItem}>(/public/MyCustodyItem, target: /storage/MyCustodyItem)
        }

        execute {

        }
    }
    ```
    2. Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.
    ``` cadence
    import ChainOfCustody from 0x01

    pub fun main(address: Address): String {
        let publicCapability: Capability<&ChainOfCustody.CustodyItem{ChainOfCustody.IPublicCustodyItem}> = getAccount(address).getCapability<&ChainOfCustody.CustodyItem{ChainOfCustody.IPublicCustodyItem}>(/public/MyCustodyItem)

        let custodyItem: &ChainOfCustody.CustodyItem{ChainOfCustody.IPublicCustodyItem} = publicCapability.borrow() ?? panic("No Capability or you don't have the right type")

        return custodyItem.itemID
    }
    ```
    ![Chapter 4 Day 2 Question 2 - Answer](/images/C4D2Q2.png)
    
    3. Run the script and access something you CAN read from. Return it from the script.
    ``` cadence
    import ChainOfCustody from 0x01

    pub fun main(address: Address): String {
    let publicCapability: Capability<&ChainOfCustody.CustodyItem{ChainOfCustody.IPublicCustodyItem}> = getAccount(address).getCapability<&ChainOfCustody.CustodyItem{ChainOfCustody.IPublicCustodyItem}>(/public/MyCustodyItem)

    let custodyItem: &ChainOfCustody.CustodyItem{ChainOfCustody.IPublicCustodyItem} = publicCapability.borrow() ?? panic("No Capability or you don't have the right type")

    return custodyItem.itemName
    }
    ```
    ![Chapter 4 Day 2 Question 3 - Answer](/images/C4D2Q3.png)