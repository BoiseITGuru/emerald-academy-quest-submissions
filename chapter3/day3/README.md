## Chapter 3 - Day 3

1. Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.
    * 
    ``` cadence
    pub contract EventTickets {

        pub var ticketsSold: @{String: EventItem}

        pub resource EventItem {
            pub let eventName: String
            pub let eventID: String
            pub let eventDate: String

            init(_eventName: String, _eventID: String, _eventDate: String) {
                self.eventName = _eventName
                self.eventID = _eventID
                self.eventDate = _eventDate
            }
        }

        init() {
            self.ticketsSold <- {}
        }

        pub fun addEventItem(eventItem: @EventItem) {
            self.ticketsSold[eventItem.eventID] <-! eventItem
        }

        pub fun removeEventItem(key: String): @EventItem {
            let eventItem <- self.ticketsSold.remove(key: key) ?? panic("Could not find ticket")
            return <- eventItem
        }

        pub fun getEventReferrence(key: String): &EventItem {
            return &self.ticketsSold[key] as &EventItem
        }

    }
    ```
2. Create a script that reads information from that resource using the reference from the function you defined in part 1.
    * 
    ``` cadence
    import EventTickets from 0x01

    pub fun main(): String {
        let ref = EventTickets.getEventReferrence(key: "event1")
        return ref.eventDate
    }
    ```
3. Explain, in your own words, why references can be useful in Cadence.
    * References have great value for allowing other people or applications to read or use your resources values without having to transfer ownership of it. 