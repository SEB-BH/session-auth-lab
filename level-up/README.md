<h1>
  <span class="headline">Intro to Mongoose Lab</span>
  <span class="subhead">Setup</span>
</h1>


## Mongoose Bonus Lab


### Instructions

This lab will be a step up from the regular mongoose lab. This will connect what we did today using mongoose with what we did yesterday with EJS.

### Set Up

- You want to work in the finished code of the ejs lab.
- If you are having issues getting set up ask Omar for help

### Connect to the Database

- In your `server.js` Add the code for connecting to the database
- Call the database: latabat

### Create the model
- Create a model folder
- Create a `Resturaunt.js` file
- In this file create the Schema and model for the Resturaunt.
- Resturaunt should have the following fields:


| Field        | Type      |  Validation  |
|--------------|-----------|------------- |
| name         | String    | required, minLength: 2, maxLength: 100 |
| isOpen | Boolean  | default: true |
| address     | String    | required |
| phone   | Number    | required |


- NOTE: We are removing the menu because we need thursdays lesson to include it
- Export the model (`module.exports`)
- import it in the `server.js`


### Seed the initial database
- Seeding is adding initial data into our application. This file is meant to be run by itself once before we even start our app
- Create a `seed.js` file
- In this file you should import mongoose and model
- it should connect to the database
- Create a `seedResturaunts()` function and this function should call `Resturaunt.insertMany()` with the following data:

```js
 [{
    name: 'Al Baik',
    isOpen: true,
    address: 'City Centre Road 4650, Manama',
    phone: 55509876
  },
  {
    name: "Haji's Traditional Cafe",
    isOpen: false,
    address: '150 شارع الحكومة, Manama',
    phone: 06892543
  },
  {
    name: 'Al Jabriya Turkish Restaurant',
    isOpen: true,
    address: 'Rd No 2643, Busaiteen',
    phone: 17330108
  },
    {
    name: 'Urban Slice Pizza',
    isOpen: false,
    address: 'Rd No 3421, Manama',
    phone: 043284832
  }
];

```

- Run the seed file using `node seed.js`
- CHECKPOINT: Check if the data is posted in the database


### READ functionality

- Now instead of using the array of objects we want to use the model to communicat with the database.
- in your `/resturaunts` route add `async` to the function and call `await Resturaunt.find()` to get all the Resturaunts
- console.log() the value to make sure the resturaunts were fetched correctly from the database
- pass the resturaunts to the ejs and in the ejs loop through them


### /returaunts/:id route

- In this route instead of using the `.find()` method on the array now we want to communicate with the database
- You should call `Resturaunt.findById()` to get the resturuaunt from the database by it's id
- pass the found resturaunt to the ejs page and display it.


FINSHED!