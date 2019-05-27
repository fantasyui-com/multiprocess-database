# multiprocess-database
Multiprocess safe JSON database with conflict detection.

Upgrade over UND with a cleaner API, written with import/export in mind.

## Programmer Friendly API

```ES6
const mp = require('multiprocess-database');

// Ensure database existence (create if does not exist, otherwise continue)
const usersDb = await mp.ensure('users', {cleanup:true});

// Upsert, Insert or update document with id alice
const {meta, data} = await mp.set("users", "alice", {name:'alice'});

// Get data from document with id alice
const {meta, data} = await mp.get("users", "alice");

// Check if document with id alice exists
const {meta, data} = await mp.has("users", "alice");

// Get all documents from database users
const {meta, data} = await mp.all("users");

// Delete, or do nothing if does not exist
const {meta, data} = await mp.del("users", "alice");

// Enumerate available databases
const databases = await mp.dir(); 

```
