![Inibase banner](./.github/assets/banner.jpg)

# Inibase

[![npmjs](https://img.shields.io/npm/dm/inibase.svg?style=flat)](https://www.npmjs.org/package/inibase) [![Node.js Version](https://img.shields.io/badge/node-18.11.0-blue)](https://nodejs.org/) [![License](https://img.shields.io/github/license/inicontent/inibase.svg?style=flat&colorA=18181B&colorB=28CF8D)](./LICENSE) [![Activity](https://img.shields.io/github/commit-activity/m/inicontent/inibase)](https://github.com/inicontent/inibase/pulse) [![GitHub stars](https://img.shields.io/github/stars/inicontent/inibase?style=social)](https://github.com/inicontent/inibase)

> File-based relational database, simple to use and can handle large data :fire:

## Features

- **Lightweight** 🪶 (~50kb)
- **Minimalist** :white_circle:
- **TypeScript** :large_blue_diamond:
- **Super-Fast** :turtle:
- **Suitable for large data** :page_with_curl:
- **Safe** :lock:
- **...** and much more :rocket:

## Usage

```js
import Inibase from "inibase";
const db = new Inibase("/database_name");

// Get all items from "user" table
const users = await db.get("user");

// Read page 2 content
const users = await db.get("user", undefined, { page: 2, per_page: 15 });

// Get only required columns to improve speed
const users = await InibaseDB.get("user", undefined, {
  columns: ["username", "address.street", "hobbies.*.name"],
});

// Get items from "user" table where "favoriteFoods" does not includes "Pizza"
const users = await InibaseDB.get("user", { favoriteFoods: "![]Pizza" });
```

If you like Inibase, please sponsor: [GitHub Sponsors](https://github.com/sponsors/inicontent) || [Paypal](https://paypal.me/KarimAmahtil).

## Sponsors

<br>
<br>

Become a sponsor and have your company logo here 👉 [GitHub Sponsors](https://github.com/sponsors/inicontent) || [paypal](https://paypal.me/KarimAmahtil).

## Install

```js
// npm
npm install inibase

// pnpm
pnpm add inibase

// yarn
yarn add inibase
```

## Examples

#### POST

```js
import Inibase from "inibase";
const db = new Inibase("/database_name");

const user_schema = [
  {
    key: "username",
    type: "string",
    required: true,
  },
  {
    key: "email",
    type: "string",
    required: true,
  },
  {
    key: "age",
    type: "number",
    required: true,
  },
  {
    key: "isActive",
    type: "boolean",
    // required: false
  },
  {
    key: "hobbies",
    type: "array",
    children: [
      {
        key: "name",
        type: "string",
        // required: false
      },
      {
        key: "level",
        type: "string",
        // required: false
      },
    ],
  },
  {
    key: "favoriteFoods",
    type: "array",
    children: "string",
    // required: false
  },
  {
    key: "address",
    type: "object",
    children: [
      {
        key: "street",
        type: "string",
        // required: false
      },
      {
        key: "city",
        type: "string",
        // required: false
      },
      {
        key: "country",
        type: "string",
        // required: false
      },
    ],
  },
];

const user_data = [
  {
    username: "user1",
    email: "user1@example.com",
    age: 25,
    isActive: true,
    hobbies: [
      { name: "Reading", level: "Intermediate" },
      { name: "Cooking", level: "Beginner" },
    ],
    favoriteFoods: ["Pizza", "Sushi", "Chocolate"],
    address: {
      street: "123 Main St",
      city: "Exampleville",
      country: "Sampleland",
    },
  },
  {
    username: "user2",
    email: "user2@example.com",
    age: 30,
    isActive: false,
    hobbies: [
      { name: "Gardening", level: "Advanced" },
      { name: "Photography", level: "Intermediate" },
    ],
    favoriteFoods: ["Burgers", null, "Salad"],
    address: {
      street: "456 Elm Rd",
      city: "Testington",
      country: "Demo Country",
    },
  },
];

const users = await db.post("user", user_data);
// [
//   {
//     "id": "1d88385d4b1581f8fb059334dec30f4c",
//     "username": "user1",
//     "email": "user1@example.com",
//     "age": 25,
//     "isActive": true,
//     "hobbies": {
//       "name": [
//         "Reading",
//         "Cooking"
//       ],
//       "level": [
//         "Intermediate",
//         "Beginner"
//       ]
//     },
//     "favoriteFoods": [
//       "Pizza",
//       "Sushi",
//       "Chocolate"
//     ],
//     "address": {
//       "street": "123 Main St",
//       "city": "Exampleville",
//       "country": "Sampleland"
//     }
//   },
//   {
//     "id": "5011c230aa44481bf7e8dcfe0710474f",
//     "username": "user2",
//     ...
//   },
//   ...
// ]
```

Link two tables: "product" with "user"

```js
import Inibase from "inibase";
const db = new Inibase("/database_name");

const product_schema = [
  {
    key: "title",
    type: "string",
    required: true,
  },
  {
    key: "price",
    type: "number",
  },
  {
    key: "user",
    type: "table",
    required: true,
  },
];

const product_data = [
  {
    title: "Product 1",
    price: 16,
    user: "1d88385d4b1581f8fb059334dec30f4c",
  },
  {
    title: "Product 2",
    price: 10,
    user: "5011c230aa44481bf7e8dcfe0710474f",
  },
];

const product = await db.post("product", product_data);
// [
//   {
//     "id": "1d88385d4b1581f8fb059334dec30f4c",
//     "title": "Product 1",
//     "price": 16,
//     "user": {
//       "id": "1d88385d4b1581f8fb059334dec30f4c",
//       "username": "user1",
//       "email": "user1@example.com",
//       ...
//     }
//   },
//   {
//     "id": "5011c230aa44481bf7e8dcfe0710474f",
//     "title": "Product 2",
//     "price": 10,
//     "user": {
//       "id": "5011c230aa44481bf7e8dcfe0710474f",
//       "username": "user2",
//       ...
//     }
//   }
// ]
```

#### GET

```js
import Inibase from "inibase";
const db = new Inibase("/database_name");

// Get "user" by id
const user = await db.get("user", "1d88385d4b1581f8fb059334dec30f4c");
// {
//     "id": "1d88385d4b1581f8fb059334dec30f4c",
//     "username": "user1",
//     "email": "user1@example.com",
//     "age": 25,
//     "isActive": true,
//     "hobbies": {
//         "name": [
//             "Reading",
//             "Cooking"
//         ],
//         "level": [
//             "Intermediate",
//             "Beginner"
//         ]
//     },
//     "favoriteFoods": [
//         "Pizza",
//         "Sushi",
//         "Chocolate"
//     ],
//     "address": {
//         "street": "123 Main St",
//         "city": "Exampleville",
//         "country": "Sampleland"
//     }
// }

// Get "user" by Criteria: where "favoriteFoods" includes "Pizza"
const users = await db.get("user", { favoriteFoods: "[]Pizza" });
// [
//   {
//     "id": "1d88385d4b1581f8fb059334dec30f4c",
//     "username": "user1",
//     "email": "user1@example.com",
//     "age": 25,
//     "isActive": true,
//     "hobbies": {
//       "name": [
//         "Reading",
//         "Cooking"
//       ],
//       "level": [
//         "Intermediate",
//         "Beginner"
//       ]
//     },
//     "favoriteFoods": [
//       "Pizza",
//       "Sushi",
//       "Chocolate"
//     ],
//     "address": {
//       "street": "123 Main St",
//       "city": "Exampleville",
//       "country": "Sampleland"
//     }
//   },
//   ...
// ]
```

#### PUT

```js
import Inibase from "inibase";
const db = new Inibase("/database_name");

// set "isActive" to "false" for all items in table "user"
await db.put("user", { isActive: false });

// set "isActive" to "true" for specific "user" by id
await db.put("user", { isActive: false }, "1d88385d4b1581f8fb059334dec30f4c");

// set "isActive" to "true" in table "user" by criteria (where "isActive" is equal to "true")
await db.put("user", { isActive: false }, { isActive: true });
```

#### DELETE

```js
import Inibase from "inibase";
const db = new Inibase("/database_name");

// delete all items in "user" table
await db.delete("user");

// delete a specific "user" by id
await db.put("user", "1d88385d4b1581f8fb059334dec30f4c");

// delete "user" by criteria (where "isActive" is equal to "false")
await db.put("user", { isActive: false });
```

## Typescript

#### Schema

```js
type FieldType =
  | "string"
  | "number"
  | "boolean"
  | "date"
  | "email"
  | "url"
  | "table"
  | "object"
  | "array";
type Field =
  | {
      id?: string | number | null | undefined,
      key: string,
      type: Exclude<FieldType, "array" | "object">,
      required?: boolean,
    }
  | {
      id?: string | number | null | undefined,
      key: string,
      type: "array",
      required?: boolean,
      children: FieldType | FieldType[] | Schema,
    }
  | {
      id?: string | number | null | undefined,
      key: string,
      type: "object",
      required?: boolean,
      children: Schema,
    };

type Schema = Field[];
```

## Roadmap

- [x] Actions:
  - [x] GET:
    - [x] Pagination
    - [x] Criteria
    - [x] Columns
    - [ ] Order
  - [x] POST
  - [x] PUT
  - [x] DELETE
- [ ] Schema supported types:
  - [x] String
  - [x] Number
  - [x] Boolean
  - [x] Date
  - [x] Email
  - [x] Url
  - [x] Table
  - [x] Object
  - [x] Array
  - [ ] Password
  - [ ] IP
- [ ] Suggest [new feature +](https://github.com/inicontent/inibase/discussions/new?category=ideas)

## License

[MIT](./LICENSE)
