## Chapter 2 - Day 2

1. Explain why we wouldn't call changeGreeting in a script.
    * changeGreeting modifies a value on the blockchain requiring gas fees and is thus a transaction.
2. What does the AuthAccount mean in the prepare phase of the transaction?
    * The AuthAccount is the user or account that is paying for and signing the transaction to the blockchain
3. What is the difference between the prepare phase and the execute phase in the transaction?
    * The prepare phase is used to access the accounts storage and the underlying data, while the execution phase is used to call your functions that update the blockchain.
4. -
    1. Contract Changes
        * Add two new things inside your contract:
            * A variable named myNumber that has type Int (set it to 0 when the contract is deployed)
            * A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber
        * 
        ``` cadence
        pub contract HelloWorld {
            pub var greeting: String
            pub var myNumber: Int

            init() {
                self.greeting = "Hello World"
                self.myNumber = 0
            }

            pub fun changeGreeting(newGreeting: String) {
                self.greeting = newGreeting
            }

            pub fun updateMyNumber(newNumber: Int) {
                self.myNumber = newNumber
            }
        }
        ```
    2. Scripts
        * Add a script that reads myNumber from the contract

         ``` cadence
        import HelloWorld from 0x01

        pub fun main(): Int {
        return HelloWorld.myNumber
        }
        ```
    3. Transactions
        * Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.

         ``` cadence
        import HelloWorld from 0x01

        transaction(myNewNumber: Int) {

            prepare(signer: AuthAccount) {}

            execute {
                HelloWorld.updateMyNumber(newNumber: myNewNumber)
            }
        }
        ```
        ![Chapter 2 Day 2 Question 4 - Answer](images/C2D2Q4.png)