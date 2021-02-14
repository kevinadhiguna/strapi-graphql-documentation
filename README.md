<div align="center">
  <h2>Strapi GraphQL API - queries & mutations</h2>
</div>
<br>

Hello there, welcome to Strapi GraphQL API documentation! This contains some of queries and mutations that hopefully helps you if you are using GraphQL API in your Strapi project :)

## Register
Just like any other applications that requires you to create an account, you have to sign up first to create a user in `users` collection type that comes default in Strapi. Here is how to register an account :
```
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
```
{
  "input": {
    "username": "YOUR_USERNAME",
    "email": "YOUR_EMAIL",
    "password": "YOUR_PASSWORD"
  }
}
```
Finally, a JWT shows in response.

## Login
```
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
```
{
  "input": {
    "identifier": "YOUR_USERNAME OR YOUR EMAIL",
    "password": "YOUR_PASSWORD"
  }
}
```
Eventually, you will get JWT in response.

## Me Query

To identify current user, you can use `me` query, like this :
```
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

### How to attach JWT in headers :
`authorization : Bearer YOUR_TOKEN`

## Create a User in Users (a collection type that comes default in Strapi app)
>What? Create a User? Did I just create a User using `Registration` mutation above?

Sure, here is some notable points :
|                                            |                `Create User` mutation                |                                   `Registration` mutation                                   |
|:------------------------------------------:|:----------------------------------------------------:|:-------------------------------------------------------------------------------------------:|
|       Needs JWT attached in Headers?       | Yes, usually you must be `superadmin` role in Strapi |                                              No                                             |
| Is created user `authenticated` initially? |                          No                          | Yes, users that are created with `Registration` mutation is already authenticated initially |

```
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
```
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

### How to get Superadmin's JWT

Go to `Documentation` in the menu on the left side -> Copy the token in `Retrieve your jwt token`.

## Create an Entry in a Collection Type

Suppose you have created a collection type named `idCardVerification`. Here is how you can add a new record inside it :
```
mutation createIdCardVerification($input: createIdCardVerificationInput) {
  createIdCardVerification(input: $input) {
    idCardVerification {
      id
      identifier
    }
  }
}

```

Variables :
```
{
  "input": {
    "data": {
      "identifier": "YOUR_USERNAME OR YOUR_EMAIL"
    }
  }
}
```

## Upload a single image

Please create a new entry in your collection type API first ! Otherwise this will not be attached to your entry.
Note : the `refId` is the ID of the entry you create in your collection type API.
```
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
```
{
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE",
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "field": "FIELD_NAME"
}
```

##  Upload multiple images in a single field
```
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
```
{
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE",
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "field": "FIELD_NAME"
}
```

<b>Note : In this case, I attached images with name `files.0`, `files.1`, ... , `files.n` as variables' names until the number of image you want to upload (n).</b>

## Upload a single image in separate fields
>Hmm... but how do I upload a single image to several fields in a single request?

All right, imagine you created a collection type which has several fields, including `cardImage`, `facePhoto`, and `personWithCardPhoto`. Otherwise, just replace those fields with yours. Ok, here we go :
```
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
```
{
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE"
}
```

<b>Please do not forget to attach your files with variables' names.</b><br>
Note: In this case, the variables' names are `cardImage`, `facePhoto`, and `personWithCardPhoto`.

### How does `UploadSingleImageToSeveralFields` mutation work ?

In the `UploadSingleImageToSeveralFields` mutation above, you still need `ref`, `refId`, and field name. However you are sending a request to a collection type and are trying to attach images in a sngle record inside the collection type. So, you are able to set `ref` and `refId` as variables. The field name ? You should name it statically as you want to upload an image in different fields. Hopefully this approach helps :)

## Get all files
>All right, I got images and files uploaded to my Strapi app but how do I know what files did I upload ?  

To get all the files uploaded to database within your Strapi app, here is the query :
```
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
