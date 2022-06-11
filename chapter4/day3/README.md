## Chapter 4 - Day 3

1. Why did we add a Collection to this contract? List the two main reasons.
    * The collection is used to better store the NFT allowing for multiple NFTs to be stored within it and also for allowing for better ussability, using interfaces and capabilities we can utilize the collection to give another user functionality within the collection.
2. What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")
    * You must explictly destroy nested resources using the `destroy` function that is called with the parent resource is destroyed.
3. Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.
    1. You could create functions at the collection level allowing users to read data about NFTs without removing them from the collection.
    2. You could utilize arrays or dictionaries to create a whitelist of accounts that can mint the NFTs
    3. Depending on use case you might also want to limit the total number of NFTs that can ever be minted.
    4. The contract has a total supply variable but it is never updated when NFTs are minted