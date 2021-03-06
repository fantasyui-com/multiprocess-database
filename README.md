# multiprocess-database
Multiprocess safe JSON database with conflict detection.

Upgrade over UND with a cleaner API, written with import/export in mind.

## Dictionary of Terms

- Database
  - Stores Objects
  - Examples: Mongo, Maria, MySQL
- Object
  - Entry in Database
  - Examples: UserAlice, Row, Document
- Property
  - Property of Object
  - Examples: userEmail, Object Property, SQL Field, Excell Cell
- Value
  - Value of Property
  - Examples: 'alice@aol.com', Cell Value

## Programmer Friendly API

```ES6

const mp = require('multiprocess-database');

```

### API Overview

```ES6

// CREATE, DELETE, UPDATE

// Upsert (Insert or Update) object with id alice.
const {meta, data} = await mp.set("users", "alice", {name:'alice'}, {deleted:false}); // also undeleted if previously deleted

// Upsert (Insert or Update) a property, merge objects.
const {meta, data} = await mp.set("users", "alice", {email:'alice@example.com'});

// GET
// Get values from object with id alice.
// NOTE: you must check if meta.deleted is true
const {meta, data} = await mp.get("users", "alice");

// CHECK
// Check if object with id alice exists.
// NOTE: you must check if data is defined
const {meta, data} = await mp.get("users", "alice");

// ALL
// Get all objects from database users.
const allArray = await mp.all("users");

// FIND
// Get matching objects from database users.
const matchingArray = await mp.all("users").filter({meta} => meta.deleted);
const matchingArray = await mp.all("users").filter({meta} => !meta.deleted);

// Finding all aol users (note you must first filter out deleted objects)
const matchingArray = await mp.all("users")
  .filter({meta} => !meta.deleted)
  .filter({data} => data.email.includes('@aol.com'));

```

## Object Structure

```ES6
{

  // Database Data
  meta: {
       uuid: '50b55281-adb2-46d0-85d0-d8a80dcc6b92',
       user: '07791d11-125b-43f7-ad27-694bb7f10a48',
    inherit: ['User', 'Authenticated', 'Animal'],
       tags: ['red', 'green', 'blue'],
    version: '132-0677083e-5edd-4e4d-8e67-388c479fec51',
      order: '0000001-c9dc5f89-b4ef-4e5b-a736-0caa9c3d0f57',
    deleted: false,
  },

  // User Data
  data: {
      'text': 'Buy Socks'
  }

}
```

## Todo

Move ensure into chain of operations
```ES6
// Ensure database existence (create if does not exist, otherwise continue)
const {meta} = await mp.ensure('users', {cleanup:true});
```

### Developer Notes

## File Storage Strategy

```ES6

const file = makeFilename(object);

// Check if the file exists in the current directory, and if it is writable.
fs.access(file, fs.constants.F_OK | fs.constants.W_OK, (err) => {
  if (err) {
    console.error(
      `${file} ${err.code === 'ENOENT' ? 'does not exist' : 'is read-only'}`);
  } else {
    console.log(`${file} exists, and it is writable`);
  }
});

```


```ES6
fs.open('myfile', 'wx', (err, fd) => {
  if (err) {
    if (err.code === 'EEXIST') {
      console.error('myfile already exists');
      return;
    }

    throw err;
  }

  writeMyData(fd);
});

```
