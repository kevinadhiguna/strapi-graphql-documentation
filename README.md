<div align="center">
  <img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/images/strapql.png" />
  <h3 align="center">Strapi GraphQL API Documentation</h3>

  <p align="center">
    Collections of GraphQL queries and mutations that power your Strapi app!
    <br />
    <a href="https://github.com/kevinadhiguna/strapi-graphql-documentation#-table-of-contents"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/kevinadhiguna/strapi-graphql-documentation/issues">Report Bug</a>
    ·
    <a href="https://github.com/kevinadhiguna/strapi-graphql-documentation/issues">Request Feature</a>
  </p>
</div>
<br>

<!-- TABLE OF CONTENTS -->
## 📚 Table of Contents

1. [About Strapql](#-about-strapql)
2. [Queries and Mutations](#-queries-and-mutations)
    - [Register](#%EF%B8%8F-register)
    - [Login](#-login)
    - [Me Query](#-me-query)
      - [How to attach JWT in headers](#-how-to-attach-jwt-in-headers-)
    - [Create a user in Users](#-create-a-user-in-users-a-collection-type-that-comes-default-in-strapi-app)
      - [How to get Superadmin's JWT](#-how-to-get-superadmins-jwt)
    - [Retrieve/Fetch a single User](#-retrievefetch-a-single-user)
    - [Retrieve/Fetch all Users](#-retrievefetch-all-users)
    - [Update a User](#-update-a-user)
    - [Delete/Remove a User](#-deleteremove-a-user)
    - [Create an Entry in a Collection Type](#-create-an-entry-in-a-collection-type)
    - [Fetch/Retrieve a single entry in collection type](#-fetchretrieve-a-single-entry-in-collection-type)
    - [Fetch/Retrieve all entries in collection type](#-fetchretrieve-all-entries-in-collection-type)
    - [Update an entry in collection type](#-update-an-entry-in-collection-type)
    - [Delete/Remove an entry in collection type](#-deleteremove-an-entry-in-collection-type)
    - [Upload a single image](#-%EF%B8%8F-upload-a-single-image)
    - [Upload multiple images in a single field](#-%EF%B8%8F-upload-multiple-images-in-a-single-field)
    - [Upload a single image in separate fields](#-%EF%B8%8F-upload-a-single-image-in-separate-fields)
      - [How does `UploadSingleImageToSeveralFields` mutation work ?](#-how-does-uploadsingleimagetoseveralfields-mutation-work-)
    - [Get all files](#-get-all-files)
    - [Fetch a single role](#-fetch-a-single-role)
    - [Fetch all roles](#---fetch-all-roles)
3. [Contributing](#-contributing)
4. [Contact](#-contact)
5. [Acknowledgements](#-acknowledgements)

<!--
<ol>
  <li>
    <a href="#-about-strapql">About Strapql</a>
  </li>
  <li>
    <a href="#-queries-and-mutations">Queries and Mutations</a>
  </li>
  <li><a href="#-contributing">Contributing</a></li>
  <li><a href="#-contact">Contact</a></li>
  <li><a href="#-acknowledgements">Acknowledgements</a></li>
</ol>
-->

## 🌟 About Strapql

Hello there, welcome to Strapi GraphQL API documentation! This contains some of queries and mutations that hopefully helps you if you are using GraphQL API in your Strapi project :)

## 🌈 Queries and Mutations

- Queries are used to read or fetch values (`READ`/`RETRIEVE`).
- Mutations modify data in the data store and returns a value. It can be used to insert, update, or delete data (`CREATE`, `UPDATE`, and `DELETE`).
<br> (Source : [TutorialsPoint](https://www.tutorialspoint.com/graphql/index.htm))

## ®️ Register
Just like any other applications that requires you to create an account, you have to sign up first to create a user in `users` collection type that comes default in Strapi. Here is how to register an account :
```graphql
mutation Register($input: UsersPermissionsRegisterInput!) {
  register(input: $input) {
    jwt
    user {
      username
      email
    }
  }
}
```

Next, put your `username`, `email`, and `password` as variables :
```json
{
  "input": {
    "username": "YOUR_USERNAME",
    "email": "YOUR_EMAIL",
    "password": "YOUR_PASSWORD"
  }
}
```
Finally, a JWT shows in response.

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🔒 Login
```graphql
mutation Login($input: UsersPermissionsLoginInput!) {
  login(input: $input) {
    jwt
    user {
      username
      email
      confirmed
      blocked
      role {
        id
        name
        description
        type
      }
    }
  }
}
```

Then enter your `identifier` and `password` as variables :
```json
{
  "input": {
    "identifier": "YOUR_USERNAME OR YOUR EMAIL",
    "password": "YOUR_PASSWORD"
  }
}
```
Eventually, you will get JWT in response.

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🙋 Me Query

To identify current user, you can use `me` query, like this :
```graphql
query MeQuery {
  me {
    id
    username
    email
    confirmed
    blocked
    role {
      id
      name
      description
      type
    }
  }
}
```
<b>Note : `me` query requires JWT attached in headers!</b>

### 📎 How to attach JWT in headers :
`authorization : Bearer YOUR_TOKEN`

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🆕 Create a User in Users (a collection type that comes default in Strapi app)
>What? Create a User? Did I just create a User using `Registration` mutation above?

Sure, here is some notable points :
|                                            |                `Create User` mutation                |                                   `Registration` mutation                                   |
|:------------------------------------------:|:----------------------------------------------------:|:-------------------------------------------------------------------------------------------:|
|       Needs JWT attached in Headers?       | Yes, usually you must be `superadmin` role in Strapi |                                              No                                             |
| Is created user `authenticated` initially? |                          No                          | Yes, users that are created with `Registration` mutation is already authenticated initially |

```graphql
mutation CreateUser($input: createUserInput) {
  createUser(input: $input) {
    user {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        id
        name
        description
        type
        permissions {
          type
          controller
          action
          enabled
          policy
          role {
            name
          }
        }
        users {
          username
        }
      }
    }
  }
}
```

Pass these variables :
```json
{
  "input": {
    "data": {
      "username": "YOUR_USERNAME",
      "email": "YOUR_EMAIL",
      "password": "YOUR_PASSWORD"
    }
  }
}
```
<b>Note : Please attach a JWT in Headers, usually Superadmin's JWT.</b>

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

### 🔑 How to get Superadmin's JWT

Go to `Documentation` in the menu on the left side -> Copy the token in `Retrieve your jwt token`.

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🧑 Retrieve/Fetch a single User

Previously, we created a new user. To retrieve a specific user inside User collection type, you can make use of this query :
```graphql
query FetchSingleUser($id: ID!) {
  user(id: $id) {
    id
    createdAt
    updatedAt
    username
    email
    provider
    confirmed
    blocked
    role {
      name
    }
  }
}
```

Variables :
```json
{
  "id": "YOUR_USER_ID"
}
```

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 👥 Retrieve/Fetch all Users

If you want to get all users in your Strapi app, this is the query you are looking for : 
```graphql
query FetchUsers {
  users {
    id
    createdAt
    updatedAt
    username
    email
    provider
    confirmed
    blocked
    role {
      name
    }
  }
}
```

You do not have to pass any variables but you may need to attach JWT in your headers (depends on your Strapi app's roles & permissions).

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🔄 Update a User

Imagine you want to change a user's email. To do such things, you should use a mutation which updates the user's data. Here is an example to change a user's email :
```graphql
mutation UpdateUser($input: updateUserInput) {
  updateUser(input: $input) {
    user {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

Then pass some variables that you would like to change (in this case, `email` field) :
```json
{
  "input": {
    "where": {
      "id": "YOUR_USER_ID"
    },
    "data": {
      "email": "YOUR_USER_EMAIL"
    } 
  }
}
```

If you want to change fields other than `email`, just replace the `email` variable.

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## ❌ Delete/Remove a User

>A user decided to no longer use my app. How do I remove him/her?

Here is a mutation that might do the task :
```graphql
mutation deleteUser($input: deleteUserInput) {
  deleteUser(input: $input) {
    user {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

Place the user ID of the user you want to remove as a variable :
```json
{
  "input": {
    "where": {
      "id": "YOUR_USER_ID"
    }
  }
}
```

<b>Note : Please carefully control which roles are able to conduct `delete` operation as it is sensitive.</b>

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

<hr />

## 👨‍💻 Let's practice CRUD operations using GraphQL !

You can think collection-type as an API generated by Strapi. Let's imagine Juventus FC, an Italian football club, as a `Juventus` collection-type. This is how it looks :<br/>

<img src="https://s6.gifyu.com/images/juventus.png" alt="Collection-Type" border="0" /><br/>

📁 The Strapi app with the `Juventus` content-type above that can be used within this docs is here : [Strapi Dockerize](https://github.com/kevinadhiguna/strapi-dockerize).<br/>
Feel free to clone or fork it !

The `position` has enumeration data type but this is how the auto-generated GraphQL schema (only for the `Juventus` content-type data) looks :
```graphql
enum ENUM_JUVENTUS_POSITION {
  GK
  DF
  MF
  FW
}

type Juventus {
  id: ID!
  _id: ID!
  createdAt: DateTime!
  updatedAt: DateTime!
  name: String
  number: Int
  age: Int
  country: String
  appearences: Int
  goals: Int
  minutesPlayed: Int
  position: ENUM_JUVENTUS_POSITION
  profpic: UploadFile
  published_at: DateTime
}
```

Now, it is clear that available options for `position` are : `GK`. `DF`, `MF`, `FW`.

## 🆕 Create an Entry in a Collection Type

For example, Juventus FC buys a new player. Here is how you can add a new record inside it :
```graphql
mutation AddSingleJuventusPlayer($input: createJuventusInput) {
  createJuventus(input: $input) {
    juventus {
      id
      _id
      createdAt
      updatedAt
      name
      number
      age
      country
      appearences
      goals
      minutesPlayed
      position
      published_at
    }
  }
}
```

Here are variables you should pass :
```json
{
  "input": {
    "data": {
      "name": "",
      "number": ,
      "age": ,
      "country": "",
      "appearences": ,
      "goals": ,
      "minutesPlayed": ,
      "position": ""
    }
  }
}
```

For instance :
```json
{
  "input": {
    "data": {
      "name": "Radu Dragusin",
      "number": 37,
      "age": 19,
      "country": "Romania",
      "appearences": 14,
      "goals": 1,
      "minutesPlayed": 1111,
      "position": "DF"
    }
  }
}
```

You will see a response like this if successful:
```json
{
  "data": {
    "createJuventus": {
      "juventus": {
        "id": "60df54f39bc9f96f94bd7db5",
        "_id": "60df54f39bc9f96f94bd7db5",
        "createdAt": "2021-02-02T18:03:31.447Z",
        "updatedAt": "2021-02-02T18:03:31.447Z",
        "name": "Radu Dragusin",
        "number": 37,
        "age": 19,
        "country": "Romania",
        "appearences": 14,
        "goals": 1,
        "minutesPlayed": 1111,
        "position": "DF",
        "published_at": "2021-02-02T18:03:31.427Z"
      }
    }
  }
}
```

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 📮 Fetch/Retrieve a single entry in collection type

To fetch an entry in your collection type, this query is probably able help you :
```graphql
query FetchSingleJuventusPlayer($id: ID!) {
  juventus(id: $id) {
    id
    _id
    createdAt
    updatedAt
    name
    number
    age
    country
    appearences
    goals
    minutesPlayed
    position
    profpic {
      name
      url
      provider
    }
    published_at
  }
}

```

Pass the ID of the record/entry you want to fetch :
```json
{
  "id": "ID_OF_ENTRY"
}
```

To illustrate :
```json
{
  "id": "60df54f39bc9f96f94bd7db5"
}
```

The response would be like :
```json
{
  "data": {
    "juventus": {
      "id": "60df54f39bc9f96f94bd7db5",
      "_id": "60df54f39bc9f96f94bd7db5",
      "createdAt": "2021-02-02T18:03:31.447Z",
      "updatedAt": "2021-02-02T18:26:53.923Z",
      "name": "Radu Dragusin",
      "number": 37,
      "age": 19,
      "country": "Romania",
      "appearences": 14,
      "goals": 1,
      "minutesPlayed": 1111,
      "position": "DF",
      "profpic": {
        "name": "dragusin.png",
        "url": "https://res.cloudinary.com/djmhwsmks/image/upload/v1625250418/dragusin_c28d444a3b.jpg",
        "provider": "cloudinary"
      },
      "published_at": "2021-02-02T18:03:31.427Z"
    }
  }
}
```

📝 Note: To be able to see `profpic` in response, you should [upload a picture](#-%EF%B8%8F-upload-a-single-image) to the field. Otherwise it is null. Here, Cloudinary is the upload provider but you can use other options such as AWS S3 too.

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 📒 Fetch/Retrieve all entries in collection type

This may get you all the entries in your collection type :
```graphql
query FetchAllJuventusPlayers {
  juventuses {
    id
    _id
    createdAt
    updatedAt
    name
    number
    age
    country
    appearences
    goals
    minutesPlayed
    position
    profpic {
      name
      url
      provider
    }
    published_at
  }
}
```

You do not pass any variables.

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🔄 Update an entry in collection type

```graphql
mutation UpdateSingleJuventusPlayer($input: updateJuventusInput) {
  updateJuventus(input: $input) {
    juventus {
      id
      _id
      createdAt
      updatedAt
      name
      number
      age
      country
      appearences
      goals
      minutesPlayed
      position
      profpic {
        name
        url
        provider
      }
      published_at
    }
  }
}
```

You may change the variable like this :
```json
{
  "input": {
    "where": {
      "id": "ID_OF_ENTRY"
    },
    "data": {
      "FIELD_1_TO_CHANGE": "NEW_VALUE_OF_FIELD_1",
      "FIELD_2_TO_CHANGE": "NEW_VALUE_OF_FIELD_2",
      "FIELD_3_TO_CHANGE": "NEW_VALUE_OF_FIELD_3"
    }
  }
}
```

In the example above, you are changing three fields. However, you can change only one field as well.  

Pretend that a player scored 3 goals in 17 appearences throughout a season. With that being said, minutes the player has played raised to 1381 minutes. You may want to do :
```json
{
  "input": {
    "where": {
      "id": "60df54f39bc9f96f94bd7db5"
    },
    "data": {
      "appearences": 17,
      "goals": 3,
      "minutesPlayed": 1381
    }
  }
}
```

The response should include the fields with the updated value :
```json
{
  "data": {
    "updateJuventus": {
      "juventus": {
        "id": "60df54f39bc9f96f94bd7db5",
        "_id": "60df54f39bc9f96f94bd7db5",
        "createdAt": "2021-02-02T18:03:31.447Z",
        "updatedAt": "2021-02-02T18:08:19.204Z",
        "name": "Radu Dragusin",
        "number": 37,
        "age": 19,
        "country": "Romania",
        "appearences": 17,
        "goals": 3,
        "minutesPlayed": 1381,
        "position": "DF",
        "profpic": {
          "name": "dragusin.png",
          "url": "https://res.cloudinary.com/djmhwsmks/image/upload/v1625250418/dragusin_c28d444a3b.jpg",
          "provider": "cloudinary"
        },
        "published_at": "2021-02-02T18:03:31.427Z"
      }
    }
  }
}
```

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## ❌ Delete/Remove an entry in collection type

```graphql
mutation removeSingleJuventusPlayer($input: deleteJuventusInput) {
  deleteJuventus(input: $input) {
    juventus {
      id
      _id
      createdAt
      updatedAt
      name
      number
      age
      country
      appearences
      goals
      minutesPlayed
      minutesPlayed
      position
      profpic {
        name
        url
        provider
      }
      published_at
    }
  }
}
```

Variables :
```json
{
  "input": {
    "where": {
      "id": "ID_OF_ENTRY"
    }
  }
}
```

To give you an idea, let's say a player is transferred to another football club. We pass the player's id as a variable this :
```json
{
  "input": {
    "where": {
      "id": "60df54f39bc9f96f94bd7db5"
    }
  }
}
```

After removed the player successfully, Strapi will send a response like this: 
```json
{
  "data": {
    "deleteJuventus": {
      "juventus": {
        "id": "60df54f39bc9f96f94bd7db5",
        "_id": "60df54f39bc9f96f94bd7db5",
        "createdAt": "2021-02-02T18:03:31.447Z",
        "updatedAt": "2021-02-02T18:26:53.923Z",
        "name": "Radu Dragusin",
        "number": 37,
        "age": 19,
        "country": "Romania",
        "appearences": 17,
        "goals": 3,
        "minutesPlayed": 1381,
        "position": "DF",
        "profpic": {
          "name": "dragusin.png",
          "url": "https://res.cloudinary.com/djmhwsmks/image/upload/v1625250418/dragusin_c28d444a3b.jpg",
          "provider": "cloudinary"
        },
        "published_at": "2021-02-02T18:03:31.427Z"
      }
    }
  }
}
```

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

<hr />

## 📤 🖼️ Upload a single image

### ⚠️ Warning : Currently Strapi's GraphQL Playground does not support file/image upload. You can use other GraphQL client to test your GraphQL upload mutation.
One of the GraphQL clients alternaive is Altair. You can download it here : https://altair.sirmuel.design/#download

Please create a new entry in your collection type API first ! Otherwise this will not be attached to your entry.
Note : the `refId` is the ID of the entry you create in your collection type API.
```graphql
mutation SingleImageUpload($refId: ID, $ref: String, $field: String, $file: Upload!) {
  upload(refId: $refId, ref: $ref, field: $field, file: $file) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Variables :
```json
{
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE",
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "field": "FIELD_NAME"
}
```

Here is an example :<br/>
<img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/gif/singleImageUpload.gif" />

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 📤 🖼️ Upload multiple images in a single field
```graphql
mutation MultipleImageUpload(
  $refId: ID
  $ref: String
  $field: String
  $files: [Upload]!
) {
  multipleUpload(refId: $refId, ref: $ref, field: $field, files: $files) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Variables :
```json
{
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE",
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "field": "FIELD_NAME"
}
```

<b>Note : In this case, I attached images with name `files.0`, `files.1`, ... , `files.n` as variables' names until the number of image you want to upload (n).</b>

Here is an example :<br/>
<img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/gif/multipleImageUpload.gif" />

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 📤 🖼️ Upload a single image in separate fields
>Hmm... but how do I upload a single image to several fields in a single request?

All right, imagine you created a collection type which has several fields, including `cardImage`, `facePhoto`, and `personWithCardPhoto`. Otherwise, just replace those fields with yours. Ok, here we go :
```graphql
mutation UploadSingleImageToSeveralFields(
  $ref: String
  $refId: ID
  $cardImage: Upload!
  $facePhoto: Upload!
  $personWithCardPhoto: Upload!
) {
  cardImage: upload(
    ref: $ref
    refId: $refId
    field: "cardImage"
    file: $cardImage
  ) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
  facePhoto: upload(
    ref: $ref
    refId: $refId
    field: "facePhoto"
    file: $facePhoto
  ) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
  personWithCardPhoto: upload(
    ref: $ref
    refId: $refId
    field: "personWithCardPhoto"
    file: $personWithCardPhoto
  ) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Variables :
```json
{
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE"
}
```

<b>Please do not forget to attach your files with variables' names.</b><br>
Note: In this case, the variables' names are `cardImage`, `facePhoto`, and `personWithCardPhoto`.

Here is an example :<br/>
<img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/gif/uploadSingleImageToSeveralFields.gif" />

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

### 🚀 How does `UploadSingleImageToSeveralFields` mutation work ?

In the `UploadSingleImageToSeveralFields` mutation above, you still need `ref`, `refId`, and field name. However you are sending a request to a collection type and are trying to attach images in a single record inside the collection type. So, you are able to set `ref` and `refId` as variables. The field name ? You should name it statically as you want to upload an image in different fields. Hopefully this approach helps :)

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 📂 Get all files
>All right, I got images and files uploaded to my Strapi app but how do I know what files did I upload ?  

To get all the files uploaded to database within your Strapi app, here is the query :
```graphql
query FetchFiles {
  files {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Unfortunately, currently Strapi does not provide a query to fetch a single file.

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 👨‍💻 Fetch a single role

Here is the query to display a single role :
```graphql
query fetchSingleRole($id: ID!) {
  role(id: $id) {
    id
    name
    description
    type
    permissions {
      id
      type
      controller
      action
      enabled
      policy
      role {
        name
      }
    }
    users {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

Variable :
```json
{
  "id": "ROLE_ID"
}
```

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 👨‍💻 👨‍💼 🧑‍🔧 Fetch all roles

Below is the query to get all roles :
```graphql
query FetchRoles {
  roles {
    id
    name
    description
    type
    permissions {
      id
      type
      controller
      action
      enabled
      policy
      role {
        name
      }
    }
    users {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🖊 Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some Amazing Queries or Mutations'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🌐 Contact
Author : Kevin Adhiguna - [@kevinadhiguna](https://linkedin.com/in/kevinadhiguna) - hi.kevinadhiguna@gmail.com

Read more on Blog : [https://about.lovia.life/docs/engineering/graphql/strapi-graphql-documentation/](https://about.lovia.life/docs/engineering/graphql/strapi-graphql-documentation/)

See on Github Gist : [https://gist.github.com/kevinadhiguna/623af7a87a629f672ca5bf7fe60a1d58](https://gist.github.com/kevinadhiguna/623af7a87a629f672ca5bf7fe60a1d58)

Project Link: [https://github.com/kevinadhiguna/strapi-graphql-documentation](https://github.com/kevinadhiguna/strapi-graphql-documentation)

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

## 🎉 Acknowledgements
* [Strapi](https://github.com/strapi/strapi)
* [Best-README-Template](https://github.com/othneildrew/Best-README-Template)

<br /> **[⬆ back to top](#-table-of-contents)** <br /><br />

[![Visits Badge](https://badges.pufler.dev/visits/kevinadhiguna/strapi-graphql-documentation)](https://github.com/kevinadhiguna)
