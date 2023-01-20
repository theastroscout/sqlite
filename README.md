# SQLite
Asynchronous Library for sqlite3

<br/>

## Installation
```
npm install @surfy/sqlite
```

## Usage

```js

// Import library
import SQLite from "@surfy/sqlite";

const sql = new SQLite('PATH TO DB FILE');
```
<br/>

## Methods
<br/>

### .run(query)
```js

await sql.run("CREATE TABLE IF NOT EXISTS test_table (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, data TEXT);");
await sql.run("INSERT OR IGNORE INTO test_table VALUES(NULL, 'Test Record', 'Data content');");

````
<br/>

### .table(table_name)
Return Table Class for query processing

```js

let table = await sql.table('test_table');

````
<br/>

### .find(match, options)
Find in a table

```js

let table = await sql.table('test_table');
let match = {
	id: 1
};

let options = {
	fields: ['id', 'name'],
	limit: 2,
	skip: 3
};

let result = table.find(match, options);

````
<br/>

### .insert(values)
Insert data into table

```js

let table = await sql.table('test_table');

// Single value
let row = {
	name: 'Test name',
	data: {
		extraOption: 'Test Data'
	}
};

let oneID = await table.insert(row);
// oneID - Array [(int) Inserted_ID]

// Multiple values
let rows = [
	{
		name: 'Test name 1',
		data: {
			extraOption: 'Test Data 1'
		}
	},
	{
		name: 'Test name 2',
		data: {
			extraOption: 'Test Data 2'
		}
	}
];

let IDs = await table.insert(rows);
// IDs - Array [(int) Inserted_ID, (int) Inserted_ID, ...]

````
<br/>

### .each(query, options, callback)
Run the SQL query with parameters and calls the callback once for each row

```js

let table = await sql.table('test_table');

let match = {
	name: 'Test name'
};

let options = {
	fields: ['id', 'name'],
	limit: 2,
	skip: 3
};

table.each(match, options, (err, row) => {
	console.log('Row', row);
});


```
<br/>

### .get(query)
Return rows or false if error occured

```js

let rows = await sql.get("SELECT * FROM test_table WHERE id=1");
console.log(rows);

````
<br/>

### .all(query)
Return rows or false if error occured

```js

let rows = await sql.all("SELECT * FROM test_table");
console.log(rows);

````
<br/>

### .truncate(table_name)
Truncate table and reset Auto Increment

```js

await sql.truncate("test_table");

````
<br/>

### .remove(DB_name)
Remove loaded DB file.

```js

let result = await sql.removeDB();

/*

@Result
	False if DB file does not exist
	True If successful
*/

````
<br/>

### .getSQLite3()
Return original SQLite3 object

```js

const sqlite3 = sql.getSQLite3();

````

<br />
<br />

## MIT License

Copyright (c) Alexander Yermolenko • [surfy.one](https://surfy.one)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.