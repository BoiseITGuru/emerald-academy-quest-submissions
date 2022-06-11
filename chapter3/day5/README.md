## Chapter 3 - Day 5

1. For today's quest, you will be looking at a contract and a script. You will be looking at 4 variables (a, b, c, d) and 3 functions (publicFunc, contractFunc, privateFunc) defined in SomeContract. In each AREA (1, 2, 3, and 4), I want you to do the following: for each variable (a, b, c, and d), tell me in which areas they can be read (read scope) and which areas they can be modified (write scope). For each function (publicFunc, contractFunc, and privateFunc), simply tell me where they can be called.
    * Variable A
        * Read Scoped
            * Area's 1, 2, 3, 4
        * Write Scoped
            * Area's 1, 2, 3, 4
    * Variable B
        * Read Scoped
            * Area's 1, 2, 3, 4
        * Write Scoped
            * Area 1
    * Variable C
        * Read Scoped
            * Area's 1, 2, 3
        * Write Scoped
            * Area 1
    * Variable D
        * Read Scoped
            * Area 1
        * Write Scoped
            * Area 1
    * publicFunc
        * Access Scoped
            * Area's 1, 2, 3, 4
    * privateFunc
        * Access Scoped
            * Area 1
    * contractFunc
        * Access Scoped
            * Area's 1, 2, 3
        