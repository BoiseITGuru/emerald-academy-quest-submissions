## Chapter 2 - Day 3

1. In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.
    * ![Chapter 2 Day 3 Question 1 - Answer](images/C2D3Q1.png)
2. In a script, initialize a dictionary that maps the Strings Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a UInt64 that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!
    *
     ``` cadence
    pub fun main() {
        let socialUssage: {String: UInt64} = {"Facebook": 1, "Instagram": 0, "Twitter": 4, "Reddit": 2, "LinkedIn": 3}
    }
    ```
3. Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).
    * The ! force unwrap operator is used to forcably unwrap optional variables. Since the operation is forced it will fail or exit the code if the value is nil.
    ``` cadence
    pub fun main() {
        var int1: Int? = 1
        var unwrappedInt1: Int = int1!  

        var int2: Int? = nil
        var unwrappedInt2: Int = int2! 
    }
    ```
4. Image Explenation
    1. What the error message means
        * The error message states that it is expecting a string to be returns not an optional
    2. Why we're getting this error
        * The variable returned is a dictionary and dictonaries always return as an optional
    3. How to fix it
        * You could fix this by using ! to force unwrap the returned value or preferably change the return type to accept the optional and allow the error to be handled in the client.
