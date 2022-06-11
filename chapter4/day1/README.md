## Chapter 4 - Day 1

1. Explain what lives inside of an account.
    * Accounts consist of any contract code deployed by your account as well as a storage section that has both public and private paths.
2. What is the difference between the /storage/, /public/, and /private/ paths?
    1. /storage/
        * This is main storage section of your accout where all your data is actually stored.
    2. /public/
        * This is a public storage section that is available to everyone
    3. /private/
        * This is a private storage section that is only available to the owner and specefic people the give access to.
3. What does .save() do? What does .load() do? What does .borrow() do?
    1. .save()
        * This saves a resource into account storage at the path specified.
    2. .load()
        * This loads a resrouce from account storage using the type and path specified.
    3. .borrow()
        * This create a reference to a resource that someone can borrow without removing the resource from your account storage.
4. Explain why we couldn't save something to our account storage inside of a script.
    * Scripts only allow you to read things from the blockchain, since saving someting would modify the blockchain it requires gas fees thus needing a transaction to complete it.
5. Explain why I couldn't save something to your account.
    * Only the account owner can access their account storage utilziing their AuthAccount.
6. Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:
``` cadence
pub contract ChainOfCustody {

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
        
    }

    pub fun createCustodyItem(_itemName: String, _itemID: String): @CustodyItem {
        return <- create CustodyItem(_itemName: _itemName, _itemID: _itemID)
    }

}
```
    1. A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.

    ``` cadence
    import ChainOfCustody from 0x01

    transaction(_itemName: String, _itemID: String) {

        prepare(signer: AuthAccount) {
            let custodyItem <- ChainOfCustody.createCustodyItem(_itemName: _itemName, _itemID: _itemID)
            signer.save(<- custodyItem, to: /storage/MyCustodyItem)

            let loadedCustodyItem <- signer.load<@ChainOfCustody.CustodyItem>(from: /storage/MyCustodyItem) ?? panic("Nothing found at this path")
            log(loadedCustodyItem.itemName)
            destroy loadedCustodyItem
        }

        execute {

        }
    }
    ```
    2. A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.

    ``` cadence
    import ChainOfCustody from 0x01

    transaction(_itemName: String, _itemID: String) {

        prepare(signer: AuthAccount) {
            let custodyItem <- ChainOfCustody.createCustodyItem(_itemName: _itemName, _itemID: _itemID)
            signer.save(<- custodyItem, to: /storage/MyCustodyItem)

            let loadedCustodyItem = signer.borrow<&ChainOfCustody.CustodyItem>(from: /storage/MyCustodyItem) ?? panic("Nothing found at this path")
            log(loadedCustodyItem.itemName)
        }

        execute {

        }
    }
    ```