## Chapter 2 - Day 4

1. Deploy a new contract that has a Struct of your choosing inside of it (must be different than Profile).
2. Create a dictionary or array that contains the Struct you defined.
3. Create a function to add to that array/dictionary.
``` cadence
pub contract Companies {
    pub var companies: {Address: Company}

    pub struct Company {
        pub let companyName: String
        pub let companyFormationDate: String
        pub let account: Address
        pub var employees: {Address: Employee}

        init(_companyName: String, _CompanyFormationDate: String, _account: Address) {
            self.companyName = _companyName
            self.companyFormationDate = _CompanyFormationDate
            self.account = _account
            self.employees = {}
        }

        pub fun addEmployee(firstName: String, lastName: String, birthday: String, account: Address) {
            let newEmployee = Employee(_firstName: firstName, _lastName: lastName, _birthday: birthday, _account: account)
            self.employees.insert(key: account, newEmployee)
        }
    }
    
    pub struct Employee {
        pub let firstName: String
        pub let lastName: String
        pub let birthday: String
        pub let account: Address

        init(_firstName: String, _lastName: String, _birthday: String, _account: Address) {
            self.firstName = _firstName
            self.lastName = _lastName
            self.birthday = _birthday
            self.account = _account
        }
    }

    pub fun addCompany(companyName: String, companyFormationDate: String, account: Address) {
        let newCompany = Company(_companyName: companyName, _CompanyFormationDate: companyFormationDate, _account: account)
        self.companies.insert(key: account, newCompany)
    }

    init() {
        self.companies = {}
    }
}
```
4. Add a transaction to call that function in step 3.
    *
    ``` cadence
    import Companies from 0x01

    transaction(companyName: String, companyFormationDate: String, companyAccount: Address, firstName: String,lastName: String, birthday: String, employeeAccont: Address) {

        prepare(acct: AuthAccount) {}

        execute {
            Companies.addCompany(companyName: companyName, companyFormationDate: companyFormationDate, account: companyAccount)

            Companies.companies[companyAccount]!.addEmployee(firstName: firstName, lastName: lastName, birthday: birthday, account: employeeAccont)
        }
    }
    ```
5. Add a script to read the Struct you defined.
    *
    ``` cadence
    import Companies from 0x01

    pub fun main(account: Address): Companies.Company {
    return Companies.companies[account]!
    }
    ```
