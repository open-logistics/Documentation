# Design 

The main intention of this project is to be able to provide everyone with a robust system that can be used to fulfil their specific requirements, this means our projects need to be maintainable by a wide array of people and companies. 

Because of this we want our designs to be accessible to that wide audience as well, with that thought in mind we must also ensure that we have no hard requriement on how the software projects are implemented, instead we will try and focus as much as possible around how our projects communicate between each other and how our software is consumed. 

We need to be able to describe our process in a couple of different ways, we want to be able to provide the signature of our software and how other software can communicate with the software, and we also need to describe how the software should react to different communications. 

To define the external signature of our software we will use [GraphQL](https://graphql.org/), this will allow us to provide a robust signature which then can be implemented across multiple languages and frameworks. 

To define the internal process of our software we will use a popular language to define a kind of pseudo code for the implementation. In the future we will look into using something like [Cucumber](https://cucumber.io/) to define business-readable specifications. 

The software may be implemented for greatly different use cases as well, which means we need to keep in mind that people maybe using an relational database, or a NoSQL database, either in memory or persisted, because of this we need to keep this in mind when defining what exactly the core functions of each service is, for example it may not be a hard requirement for a company to provide full text search across their data, while others might have this requirement. This means that we need to define our core functions as a basic CRUD system where we make no assumptions on the backing database.

Before we create a design we must have identified a use case where the API is going to be consumed, we will be able to create multiple processes that use the same designs, however a design without a use case is never going to be used for a general consumer. 

## Example

### GraphQL

_An example implementation of this can be found [here](https://github.com/open-logistics/simple-graphql-example-nodejs)_

Here we provide a basic framework for keeping track of inventory for a specific items at a location.

First we need an item that contains basic information about our items,

For our items we only want to record the count of available items, for this example we can also go into negative quantities, which would allow for [back orders](#Terms-used). 

We don't need to record any information about what the item is, or how what quantity is measured in, we only want to know about **how much** of the item we have on hand. 

We want to record the location of each item though, as each location may have a different amount of quantities.

```graphql
type InventoryItem {
	key: String!
	location: String!
	quantity: Int!
}
```

We need also to define an input definition for our type so we can use our type in mutation requests, this will mimic the information we output:

```graphql
input InventoryItemInput {
	key: String!
	location: String!
	quantity: Int!
}
```

Now that we have defined our type, we want to be able to tell the software to _manage_ our item, this is the same as saying we have a specific item, at a specific location, with a given quantity, if we don't know the quantity then `0` can be provided.

We will call this `saveInventoryItem` which will persist the item in our database, it will accept one parameter that is called `data`, and expects a _non-null_ `InventoryItemInput` object. We are going to return our `InventoryItem` that we just saved, this will be non-nullable as we will always expect it to be present. 

```graphql
type Mutation {
	saveInventoryItem(data: InventoryItemInput!): InventoryItem!
}
```

Now with this `saveInventoryItem` we can create and update our item. 

We want to be able to modify the quantity we have on hand for our item, if we want we can read our item from the database, modify the value, and send it back to the database, however this may mean that someone could change it in the mean time, instead we are going to provide another function called `incrementInventoryItem` which accepts the same parameter as `saveInventoryItem`, and returns the same data. Instead of replacing what we have in memory, we are going to change `quantity` by the amount that was provided, this allows us to both increment by a positive number or negative number. 

This leaves us with a very simple mutation type:

```graphql
type Mutation {
	saveInventoryItem(data: InventoryItemInput!): InventoryItem!
	incrementInventoryItem(data: InventoryItemInput!): InventoryItem!
}
```

At some point we may want to no longer retain an inventory item and we will want to get rid of it, for this we will want a delete mutation function, this function will accept two parameters `key: String!`, and `location: String!`, this will return a `InventoryItem`, which will contain the last value of the item, if it had already been deleted the function will return null. This gives us mutation type:

```graphql
type Mutation {
	saveInventoryItem(data: InventoryItemInput!): InventoryItem!
	incrementInventoryItem(data: InventoryItemInput!): InventoryItem!
	deleteInventoryItem(key: String!, location: String!): InventoryItem
}
```

The final thing we need to define is how to get an inventory item, for this we will define a function `getInventoryItem` which will accept two parameters `key: String!`, and `location: String!`, if the item doesn't exist we will return null, if it does we will return the item:

```graphql
type Query {
	getInventoryItem(key: String!, location: String!): InventoryItem
}
```

We may also want to list all the items we are storing information for, for this we will define the function `listInventoryItems`, which will accept a single parameter `location: String` which can be null, the function will return a list of inventory items. If the `location` parameter is provided, the returned items will be filtered so only that locations items are contained.

This leaves us with the complete schema definition, which can be seen as our services external signature:

```graphql
type InventoryItem {
	key: String!
	location: String!
	quantity: Int!
}

input InventoryItemInput {
	key: String!
	location: String!
	quantity: Int!
}

type Mutation {
	saveInventoryItem(data: InventoryItemInput!): InventoryItem!
	incrementInventoryItem(data: InventoryItemInput!): InventoryItem!
	deleteInventoryItem(key: String!, location: String!): InventoryItem
}

type Query {
	getInventoryItem(key: String!, location: String!): InventoryItem
	listInventoryItems(location: String): [InventoryItem!]!
}
```

We now have a very simple API that can be implemented in any language with some very simple rules, this is just an example so it isn't exactly how you should implement this, but gives a good idea of what we are thinking and how we would like to see projects defined.

We have tried to make no assumption about the end use case of the software and tried to stick to generic methods that can be used to only manage quantities of inventory stored an a specific location. 

## Terms used

<details>
  <summary>Back Order</summary>
  Place an order for (a product) that is temporarily out of stock.
</details>

## Licence

The written documentation and source code examples are licensed under the [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) license.