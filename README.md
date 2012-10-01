## Node SQL Query Builder

This is adapter based SQL query builder. This means that building queries for every DB like MySQL, SQLite is handled by separate adapter. It make this sql query builder highly extendable.

I have tryied to make usage quite straight forward.

### Selects

```js
var Select = require('../index').select;
var Sql    = require('../index').renderer;

// you can assign some query data on construction
var query = new Select({
	group:'time', 
	having:['t = 1 ASC', 'b = 2 DESC']
});

// You can use chains
query
	.select('u.*')
	.from('users AS u');

// put array or string or object
query.where("u.name = 'john'");

// set where to be deleted later
query.select('(SELECT COUNT(*) FROM messages) AS messages)', 'selmsg');

query.select('a.avatar').join('user_attr AS a ON a.user_id = u.id');

// show 10 items per page and 5th page
query.offset(5, 10);

// we decided not to calculate messages
query.resetSelect('selmsg');

// render to string throu renderer
var sql = Sql.toString(query);
console.log(sql);
```

### Delete

```js
var Delete = require('../index').delete;
var Sql    = require('../index').renderer;

// add parametres on cunstruction
var query1 = new Delete({from: 'users', where:["id = 10", "blocked = 0"]});
console.warn(Sql.toString(query1));

var query2 = new Delete();

// use chanes
query2.from('users').where('id = 10');

// add key to value later can delet by key
query2.where('blocked = 1', 'c1');

// put array
query2.where(['a = 1', 'a = 2']);

// put object
query2.where({test:'my = 2'});

// reset key. If no key all where reseted
query2.resetWhere('c2');

console.warn(Sql.toString(query2));
```