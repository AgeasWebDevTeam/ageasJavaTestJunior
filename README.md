# Java tech test #
## Introduction
Your task, is to build a REST API for the application provided in this project.
In order to open the web application simply open the ./public/index.html file in your preferred browser of choice.

The web application is a stock management app that should display a list of items that
can be added, amended, sold and reordered. The front end has been set up to call 
various endpoints which you will need to create. The below documentation details what your endpoints 
should return in order for the front end to work correctly.

There is deliberately no validation or special character handling on the front end.
This is not indicative of how we normally write our code.

#### POST http://localhost:8080/api/item
This endpoint gets called when adding a new item by clicking on the plus icon. Each item must have a unique id, 
how you decide this value is up to you (sequential, uuid, randomly generated etc.).

##### Request
```
POST http://localhost:8080/api/item

{
    "name": "Banana",
    "quantity": 5,
    "targetStockLevel": 15,
    "warningStockLevel": 5,
}
```

##### Responses
###### Success
```
HTTP/1.1 200 OK
```

###### Failure
Feel free to customize the messages in the array
```
HTTP/1.1 4**

{
    "messages": ["Could not create new item", "You did not enter an item name"]
}
```

#### GET http://localhost:8080/api/item
This should retrieve a list of all items. This is called on page load and when any modal is closed/submitted

The stockWarning boolean should be set to true when the quantity is less than or equal to the warningStockLevel.

##### Request
```
GET http://localhost:8080/api/item
```
##### Responses
###### Success
```
HTTP/1.1 200 OK

[
    {
        id: 0,
        name: 'Banana',
        quantity: 5,
        targetStockLevel: 15,
        warningStockLevel: 5,
        stockWarning: true
    }, 
    {
        id: 1,
        name: 'Apple',
        quantity: 100,
        targetStockLevel: 80,
        warningStockLevel: 50,
        stockWarning: false
    }, 
    {
        id: 2,
        name: 'Orange',
        quantity: 6,
        targetStockLevel: 15,
        warningStockLevel: 8,
        stockWarning: true
    }
]
```

###### Failure
Feel free to customize the messages in the array
```
HTTP/1.1 4**

{
    "messages": ["Could not retrieve list"]
}
```

#### PATCH http://localhost:8080/api/item/{id}
This endpoint is called when an item is edited. The id in the url refers to the item's id.

##### Request
```
PATCH http://localhost:8080/api/item/2

{
        name: 'Orange',
        quantity: 6,
        targetStockLevel: 20,
        warningStockLevel: 10
}
```

##### Responses
###### Success
```
HTTP/1.1 200 OK
```

###### Failure
Feel free to customize the messages in the array
```
HTTP/1.1 4**

{
    "messages": ["Could not edit item"]
}
```

#### POST http://localhost:8080/api/stock/reduce
This request is made when the "SELL ITEMS" button is clicked.

##### Request
```
POST http://localhost:8080/api/stock/reduce

[
    {
        id: 0,
        amountToSell: 3
    }, 
    {
        id: 2,
        amountToSell: 4
    }
]
```

##### Responses
###### Success
```
HTTP/1.1 200 OK
```

###### Failure
Feel free to customize the messages in the array
```
HTTP/1.1 4**

{
    "messages": ["Unable to reduce the stock", "You entered HELLO WORLD but we expected a numerical value"]
}
```

#### POST http://localhost:8080/api/stock/order
This endpoint is called when clicking on the Generate order icon.
It represents preparing an order to place with your suppliers.

You should generate a list of items that fall below the warning level and supply the quantity needed
to boost them up to the target level. This list should be stored against a unique order reference, how you decide
this value is up to you (sequential, uuid, randomly generated etc.).


##### Request
```
POST http://localhost:8080/api/stock/order
```

##### Responses
###### Success
```
HTTP/1.1 200 OK
[
    {
        id: 0,
        name: 'Banana',
        quantityNeeded: 10
    }, 
    {
        id: 2,
        name: 'Orange',
        quantityNeeded: 9
    }
]
```

###### Failure
Feel free to customize the messages in the array
```
HTTP/1.1 4**

{
    "messages": ["Unable to generate order"]
}
```

#### GET http://localhost:8080/api/stock/order
This endpoint is called when clicking on the receive order icon.

A list of unfulfilled order references that were previously generated should be returned from this endpoint. 

##### Request
```
GET http://localhost:8080/api/stock/order
```

##### Responses
###### Success
```
HTTP/1.1 200 OK
[
    "345231325",
    "940283453"
]
```

###### Failure
Feel free to customize the messages in the array
```
HTTP/1.1 4**

{
    "messages": ["Unable to retrieve order references"]
}
```

#### POST http://localhost:8080/api/stock/order/{orderRef}
The order ref is the selected reference on the receive order modal.
This represents restocking your warehouse with fresh supplies.
The endpoint is called by clicking on the FULFIL ORDER button.

##### Request
```
POST http://localhost:8080/api/stock/order/940283453
```

##### Responses
###### Success
```
HTTP/1.1 200 OK
```

###### Failure
Feel free to customize the messages in the array
```
HTTP/1.1 4**

{
    "messages": ["Unable to order new stock", "Order reference 98765 was not found"]
}
```
