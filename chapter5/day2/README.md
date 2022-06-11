## Chapter 5 - Day 2

1. Explain why standards can be beneficial to the Flow ecosystem.
    * Standards are extremely important to all digital systems and networks. The internet itself is nothing more that a set of communication standards widely adopted allowing for near universal communication. Without standards every developer is left to create their own implementation making it impossible for other developers to create applications that can consume the informations since every implementation is different. In the simplest form Standards give us a way to implement things one time and know that it will work as long as everyone else is also following the standard.
2. What is YOUR favourite food?
    * hands down it's apple bbq chicken wings.
3. Code Correction
    ``` cadence
    pub contract interface ITest {
        pub var number: Int
        
        pub fun updateNumber(newNumber: Int) {
            pre {
            newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
            }
            post {
            self.number == newNumber: "Didn't update the number to be the new number."
            }
        }

        pub resource interface IStuff {
            pub var favouriteActivity: String
        }

        pub resource Stuff {
            pub var favouriteActivity: String
        }
    }
    ```

    ``` cadence
    pub contract Test: ITest {
        pub var number: Int
        
        pub fun updateNumber(newNumber: Int) {
            self.number = newNumber
        }

        pub resource interface IStuff {
            pub var favouriteActivity: String
        }

        pub resource Stuff: IStuff {
            pub var favouriteActivity: String

            init() {
            self.favouriteActivity = "Playing League of Legends."
            }
        }

        init() {
            self.number = 0
        }
    }
    ```