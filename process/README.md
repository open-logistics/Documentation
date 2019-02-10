# Processes

Here we will define all the processes and use cases that can be used within our logistics ecosystem. 

A process doesn't need to be specifically about fulfilment etc, it can be broad, as long as it enriches the ecosystem. 

For example a process may be the way an invidaul logs in to a site, or how we identify a specific product, or even how a open source barcode is decoded so that the information is accessible, if we can define it in a well _defined_ process then we are good to go!

Following this style of development we will eliminate room for unthoughtout decisions and allow decision to be placed on practices that we do within the logistics industry. 

## Structure

In this table of contents we will outline some generic processes that we want to define to start with, if the process is linked then we have provided the details of the process that we want to fulfil. 

We will try and outline a generic topic that the process fulls under, but a process does not need to cross only one topic, so either list in both or mention the other topic.

### Types

We want to talk in a consistent manner when defining our processes, we could use real world names for things like “Person” or “Them” or “They”, but this could be confusing, so instead we want to talk about “Types”, a Person/Them/They all relate to 1 or more Individuals, with a specific set of associations that they have, for example if we had the type of `Individual`, that is part of a `Party` which identifys as an `Organisation`, an `Organisation` has multiple `Individual`s associated to it by either `Role`s or association (for example a `Consigment`s target `Party` is an `Individual` located at a `Location`, vs `Organisation` at a `Location`). By talking in types we reduce the need processes for a process that could just be a single process (for example delivery is to a `Party`, which covers both a `Individual` or `Organisation` being billed for a specific item, when an `Organisation` pays for something they may be invoiced, or where an `Individual` pays for something they would use an electronic transaction like using a credit card, the process which each `Party` takes is just an extended process seperately).

We can identify a generic process and then break it down into multiple sub processes, for example if we said:

> As an `Individual` I want to log in, so that a `Organisation` can identify me when associating the `Individual` with an `Order`

This could be broken down into multiple sub processes, for example logging in, recording a token that will be used while associating the party, providing the token to the organisation via something like a login form, then the organisation will just provide what they need for the party, like they need the address to associate to the order, or they need a shipping label associated to the order. 

For the order process to take place the fulfilling organisation doesn't need to know anything about that individual, given that their process does not require contacting the individual or organisation. 

By following this approach we also force a trust relationship within our code, each process must be transparent to the end party, for example an `Organisation` will want to only provide the required details to an integrator to receive their items, but an organisation may also need to provide very specific `Configuration` of a product, for example if they were `Order`ing a bulk amount of `Item`s to build a house from, every specific `Item` was selected by another `Organisation` (like an architect), but this entire `Order` is `Fulfill`ed with a single `Order` and fulfilled in a specific way (like all at once, or day by day, with possibility of `Schedule` changes etc). 

This style of thinking is more like thinking from the view of every party involved, and defines the complete touchpoints before something is even made. 

If we are looking at rules and regulations that are being introduced over time we know that we want tighter measures around our data, and how other organisations and individuals are using our data, by defining our processes we can also provide the Owner of information with very very precise controls on their own data. 

For example to have a `Service` provided you must have a valid `Payment Method`, an `Organisation` could stop the payment method, however the `Service` will be paused, by defining this we do two things, if an `Organisation` no longer wants to provide the payment method then `Service` will no longer have the required permission to charge the `Payment` to the `Payment Method`, we also are saying this `Service` needs to be paid for, if it can't be just stop the `Service`. 

If a `Party` says they only want to continue receiving `Service` from an `Organisation` for a specific `Process` then that termination event can be `Schedule`d... 

We want to be able to provide the most effective and open ecosystem possible and we feel like this is the best way to do so.

## TOC

- Identity
	- Identifying an individual
	- Associating an individual with an association
- Product catalog
	- Record possible catalog for catalog management 
	- List available range within catalog
	- List range for specific date for when ordering for the future
	- List range that are available now
- Ordering
	- Create an order and request an organisation to fulfil it
	- Request information about an orders status
		- Track an order with an external provider (4PL, 3PL, Courier)
	- Change an orders fulfilment address
	- Change an orders payment method
	- Provide information to an external provider
- Shipping
	- Track a courier identifier
	- Request a pickup
	- Provide identifier location information
		- Provide polled location information
	- List identifier location information

