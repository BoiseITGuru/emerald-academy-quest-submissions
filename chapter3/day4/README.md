## Chapter 3 - Day 4

1. Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)
    * Interfaces can be used to both define what variables or functions a struct or resrouce should have, you can also use interfaces to limit what variables or functions are accesible to users.
2. Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.
    * 
    ``` cadence
    pub contract EventTickets {

        pub var ticketsSold: @{String: EventItem}

        pub resource interface IEventItem {
            pub let eventName: String
            pub let eventID: String
            pub var eventDate: String
        }

        pub resource EventItem: IEventItem {
            pub let eventName: String
            pub let eventID: String
            pub var eventDate: String

            init(_eventName: String, _eventID: String, _eventDate: String) {
                self.eventName = _eventName
                self.eventID = _eventID
                self.eventDate = _eventDate
            }

            pub fun updateEventDate(newDate: String) {
                self.eventDate = newDate
            }
        }

        init() {
            self.ticketsSold <- {}
        }

        pub fun notRestricting() {
            let eventItem: @EventItem <- create EventItem(_eventName: "Test Event", _eventID: "123", _eventDate: "06/08/2022")
            eventItem.updateEventDate(newDate: "01/01/0000")
            log(eventItem.eventDate)

            destroy eventItem
        }

        pub fun restricting() {
            let eventItem: @EventItem{IEventItem} <- create EventItem(_eventName: "Test Event", _eventID: "123", _eventDate: "06/08/2022")
            eventItem.updateEventDate(newDate: "01/01/0000") //ERROR - member of restricted type is not accessible: updateEventDate
            log(eventItem.eventDate)

            destroy eventItem
        }
    }
    ```
3. Code Fix
    * 
    ``` cadence
    pub contract Stuff {

        pub struct interface ITest {
        pub var greeting: String
        pub var favouriteFruit: String
        pub fun changeGreeting(newGreeting: String): String
        }

        pub struct Test: ITest {
        pub var greeting: String
        pub var favouriteFruit: String

        pub fun changeGreeting(newGreeting: String): String {
            self.greeting = newGreeting
            return self.greeting
        }

        init() {
            self.greeting = "Hello!"
            self.favouriteFruit = "Apple"
        }
        }

        pub fun fixThis() {
        let test: Test{ITest} = Test()
        let newGreeting = test.changeGreeting(newGreeting: "Bonjour!")
        log(newGreeting)
        }
    }
    ```