# multiprocess-database
Multiprocess safe JSON database with conflict detection.

Upgrade over UND with a cleaner API, written with import/export in mind.

## Programmer Friendly API

```ES6

const mp = require('multiprocess-database');

// Ensure database existence (create if does not exist, otherwise continue)
const {meta} = await mp.ensure('users', {cleanup:true});

```

### Document Structure

```ES6

// CREATE, DELETE, UPDATE
// Upsert, Insert or update document with id alice
const {meta, data} = await mp.set("users", "alice", {deleted:false}, {name:'alice'});

// GET
// Get data from document with id alice
// NOTE: you must check if meta.deleted is true
const {meta, data} = await mp.get("users", "alice");

// CHECK
// Check if document with id alice exists
// NOTE: you must check if data is defined
const {meta, data} = await mp.get("users", "alice");

// ALL
// Get all documents from database users
const {meta, data} = await mp.all("users");


```

## Document Structure

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
