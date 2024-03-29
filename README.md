# SurrealDB Node.js

```shell
npm i surrealdb
```

## Quick Start

```sh
surreal start --root user --pass user
```

```ts
import SurrealDB from 'surrealdb'

const Surreal = new SurrealDB('http://127.0.0.1:8000', {
  user: 'root',
  pass: 'root',
  database: 'test',
  namespace: 'test',
})
```

## Guide


```ts
// Async
const Async = await Surreal.Query(`CREATE company:surrealdb SET name = 'SurrealDB', cofounders = [person:tobie, person:jaime];`)

// Promise
Surreal.Query(
  `CREATE company:surrealdb SET name = 'SurrealDB', cofounders = [person:tobie, person:jaime];`,
)
  .then((data) => {
    console.log(data)
  })
  .catch((err) => {
    console.log(err)
  })
```

## Surreal Query Builder

```ts
import { SurrealQueryBuilder } from 'surrealdb'

const SQB = new SurrealQueryBuilder()
    .DefineParam('username', 'John Doe')
    .AppendCreate('user', [
      {
        key: 'name',
        type: 'default',
        value: '$username',
      },
      {
        key: 'age',
        type: 'int',
        value: 18,
      },
      {
        key: 'friends',
        type: 'array',
        value: [
          {
            name: 'James',
            age: 17,
          },
          {
            name: 'Bob',
            age: 18,
          },
        ],
      },
    ])
    .WrapTransaction('COMMIT')

console.log(SQB.query) // OR SQB.Finalize()
```

```sql
BEGIN TRANSACTION;
LET $username = 'John Doe';
CREATE user SET name = $username, age = <int> 18, friends = [
  {
    name: 'James',
    age: 17
  },
  {
    name: 'Bob',
    age: 18
  }
];
COMMIT TRANSACTION;
```