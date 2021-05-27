---
layout: post
title: Complete example of a CRUD API with Express GraphQL
description: "CRUD API example implemented with GraphQL-js and expresss-graphql acting as integration point to the existing REST API supporting Bookmarks.dev"
author: ama
permalink: /ama/complete-example-crud-api-express-graphql
published: true
categories: [tutorial]
tags:
    - graphql
    - expressjs
    - api-design
    - rest
    - node.js
---

As you might recall from my previous post [GraphQL Resources to help you get started]({% post_url 2020-11-05-graphql-resources-to-help-you-get-started %}),
 I have started to dig deeper into GraphQL. What better way to deepen one's knowledge than with a hands-on experience?
  So, in this blog post I will present the implementation of a GraphQL server API that provides CRUD operations.
   I chose the Javascript implementation of GraphQL, GraphQL-js [^1] and set up a GraphQL server with Express Graphql[^2].
  To make the scenario more realistic, the API developed in GraphQL acts as integration layer to the existing [REST API](https://www.codever.land/api/docs/)
   supporting [Bookmarks.dev](https://wwww.bookmarks.dev).

[^1]: <https://github.com/graphql/graphql-js>
[^2]: <https://github.com/graphql/express-graphql>

> You can find the source code for the examples in this post on [Github: graphql-expressjs-crud-demo](https://github.com/CodepediaOrg/graphql-express-crud-demo)

* TOC
{:toc}

<!--more-->

<hr/>

## Configure the [demo project](https://github.com/CodepediaOrg/graphql-express-crud-demo) to test along
If you want to test along please follow the steps below:

### Setup Bookmarks.dev localhost REST API
You need to first set up the localhost REST api of Bookmarks.dev. Checkout the [project](https://github.com/codeverland/codever) and
 then follow the steps listed in the [README](https://github.com/codeverland/codever#readme) file of the project.

### Install and run the  project `graphql-expressjs-crud-demo`
To run the actual GraphQL project you need to set it up as described in the [README file](https://github.com/CodepediaOrg/graphql-express-crud-demo#readme) of the project.

### GraphiQL access
Once you are done with the setup, you can input your GraphQL queries with [GraphiQL](https://github.com/graphql/graphiql)
by accessing the [http://localhost:4000/graphql](http://localhost:4000/graphql) url in your favorite browser

<hr/>

The coming sections will present main elements of GraphQL with concrete examples and their implementation in GraphQL-js.

## Schema
Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service.
 Then, when queries come in, they are validated and executed against that schema.
 Bellow you can find some of the most common types:

### Object types and fields
The most basic components of a GraphQL schema are object types, which just represent a kind of object you can fetch
 from your service, and what fields it has. In the GraphQL schema language, we might represent it like this:

```
type Bookmark {
  _id: ID!
  userId: ID!
  public: Boolean
  location: String!
  name: String!
  description: String
  tags: [String!]!
  likeCount: Int
  sourceCodeURL: String
}
```

The language is pretty readable, but let's go over it so that we can have a shared vocabulary:

* `Bookmark` is a GraphQL Object Type, meaning it's a type with some fields. Most of the types in your schema will be object types.
* `String`, `Boolean` and `Int` are some  of the built-in scalar types - these are types that resolve to a single scalar object, and can't have sub-selections in the query. We'll go over scalar types more later.
* `ID`: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache.
 The ID type is serialized in the same way as a String; however, defining it as an ID signifies that it is not intended to be human‐readable.
* `String!` means that the field is non-nullable, meaning that the GraphQL service promises to always give you a value when you query this field.
 In the type language, we'll represent those with an exclamation mark.
* `[String!]!` represents an array of String objects. Since it is also non-nullable,
 you can always expect an array (with zero or more items) when you query the `tags` field. And since `String!` is also non-nullable,
  you can always expect every item of the array to be an String object.


The implementation in graphql-js looks something like the following:

```javascript
const Bookmark = new GraphQLObjectType({
    name: "Bookmark",
    fields: {
        _id: {
            type: GraphQLID,
            description: "The id of the bookmark it's generated in MongoDb"
        },
        userId: {
            type:  GraphQLNonNull(GraphQLID),
            description: "The id of the user that created the bookmark"
        },
        public: {
            type: GraphQLBoolean,
            description: "Whether the bookmark is public or not"
        },
        location: {
            type:  GraphQLNonNull(GraphQLString),
            description: "Mostly the URL of the link"
        },
        name: {
            type: GraphQLNonNull(GraphQLString),
            description: "Title of the bookmark"
        },
        description: {
            type: GraphQLString,
            description: "Notes about the bookmark - supports Markdown"
        },
        tags: {
            type:  GraphQLNonNull(GraphQLList(GraphQLNonNull(GraphQLString))),
            description: "Tags are highly used on Bookmarks.dev"
        },
        likeCount: {
            type: GraphQLInt,
            description: "Number of public likes"
        },
        sourceCodeURL: {
            type: GraphQLString,
            description: "Where you can find the source code related to bookmark"
        }
    }
});
```

### Arguments
Every field on a GraphQL object type can have zero or more arguments, for example the `history` field below:
```
type User {
 userId: ID!
 history(last: Int = 5): [Bookmark]
}
```

**All arguments are named**. Unlike languages like JavaScript and Python where functions take a list of ordered arguments,
 all arguments in GraphQL are passed by name specifically. In this case, the `history` field has one defined argument, `last`.

Arguments can be either required or optional. When an argument is optional, we can define a default value
- if the `last` argument is not passed, it will be set to 5 by default.

The example above looks in GraphQL-js the following - focus on the `history` field of the `User` object part:
```javascript
const User = new GraphQLObjectType({
    name: "User",
    fields: {
        userId: {
            type: GraphQLID,
            description: "user identifier - keycloak ID"
        },
        history: {
            type: new GraphQLList(Bookmark),
            description: "Bookmarks the user created, updated or clicked recently",
            args: {
                last: {
                    type: GraphQLInt,
                    defaultValue: 5,
                    description: "Fetches only *last* bookmarks from history "
                }
            },
            resolve: async (root, args, context) => {
                const userId = root.userId;
                const bearerToken = context.bearerToken;
                const last = args.last;
                const response = await bookmarksApiService.getBookmarksOfUserHistory(userId, bearerToken, last);

                return response.body;
            }
        }
    }
});
```

> Note the `resolve` method for now, but we'll talk about that later

### Enumeration types
Also called Enums, enumeration types are a special kind of scalar that is restricted to a particular set of allowed values. This allows you to:

1. Validate that any arguments of this type are one of the allowed values
2. Communicate through the type system that a field will always be one of a finite set of values

Here's what an `enum` definition might look like in the GraphQL schema language:

enum OrderBy {
  MOST_LIKES
  LAST_CREATED
  MOST_USED
}

This means that wherever we use the type `OrderBy` in our schema, we expect it to be exactly one of `MOST_LIKES`, `LAST_CREATED`, or `MOST_USED`.

In the Javascript GraphQL the definition of the enum looks like the following:
```javascript
const BookmarkOrderByType = new GraphQLEnumType({
    name: 'OrderBy',
    values: {
        MOST_LIKES: {value: "MOST_LIKES"},
        LAST_CREATED: {value: "LAST_CREATED"},
        MOST_USED: {value: "MOST_USED"}
    }
});
```

> By using the `value` parameter we force the serialization to these values.

## Queries - the R in CRUD
Queries are the bread and butter of GraphQL. You define the queries in the schema that your GraphQL provides under
the root 'Query' object:

```
type Query {
    publicBookmarks: [Bookmark]
    user(userId: ID!): [User]
    bookmark(bookmarkId: ID!): [Bookmark]
}
```

translated to the GraphQL javascript implementation:

```javascript
const Query = new GraphQLObjectType({
    name: 'Query',
    fields: {
        publicBookmarks: {
            type: new GraphQLList(Bookmark),
            resolve: async (root, args, context, info) => {
                const response = await bookmarksApiService.getPublicBookmarks();
                return response.body;
            }
        },
        userFeedBookmarks: {
            type: new GraphQLList(Bookmark),
            resolve: async (root, args, context, info) => {
                const {userId, bearerToken} = context;
                const response = await bokmarksApiService.getBookmarksForFeed(userId, bearerToken);
                return response.body;
            }
        },
        user: {
            type: User,
            args: {
                userId: {type: GraphQLID}
            },
            resolve: async (root, args, context) => {
                const bearerToken = context.bearerToken;
                const {userId} = args;
                const response = await bookmarksApiService.getUserData(userId, bearerToken);

                return response.body;
            }
        },
        bookmark: {
            type: Bookmark,
            args: {
                bookmarkId: {type: GraphQLID}
            },
            resolve: async (root, args, context, info) => {
                const bearerToken = context.bearerToken;
                const {bookmarkId} = args;
                const response = await bookmarksApiService.getBookmarkById(userId, bearerToken, bookmarkId);

                return response.body;
            }
        }
    },
});
```

Let's see now how a query would look on the client side, for example to receive data for the mock user provided by the bookmarks.dev setup:

```
{
	user(userId:"a7908cb5-3b37-4cc1-a751-42f674d870e1") {
    userId,
    profile {
      displayName
      imageUrl
    },
    bookmarks(orderBy:LAST_CREATED) {
      ...bookmarkFields
    },
    feed  {
      ...bookmarkFields
    },
    history {
      ...bookmarkFields
    }
  }
}

fragment bookmarkFields on Bookmark {
  _id
  name
  location
  tags
  sourceCodeURL
  likeCount
}
```

> Note the `fragment` construct - Fragments let you construct sets of fields, and then include them in queries where you need to,
> to not have to repeat yourself.

The response should look something similar to the following:
```json
{
  "data": {
    "user": {
      "userId": "a7908cb5-3b37-4cc1-a751-42f674d870e1",
      "profile": {
        "displayName": "Mock",
        "imageUrl": "https://gravatar.com/avatar/bc461041c4caf5493530db7a69d4bf83?s=340"
      },
      "bookmarks": [
        {
          "_id": "5fa8db1897519f34ae94f7e2",
          "name": "Build a CRUD functionality with GraphQL and ExpressJS",
          "location": "https://www.codepedia.org/ama/complete-example-crud-api-express-graphql",
          "tags": [
            "graphql",
            "expressjs",
            "graphql-express",
            "rest",
            "api-design"
          ],
          "sourceCodeURL": "https://github.com/CodepediaOrg/graphql-express-crud-demo",
          "likeCount": null
        },
        {
          "_id": "5e9d4a463b837e57e76de0ae",
          "name": "Getting started with www.codever.land",
          "location": "https://www.codever.land/howto",
          "tags": [
            "programming",
            "resource",
            "blog",
            "open-source"
          ],
          "sourceCodeURL": "https://github.com/CodepediaOrg/bookmarks",
          "likeCount": 0
        },
        {
          "_id": "5e9d4a463b837e57e76de0ad",
          "name": "Collection of public dev bookmarks, shared with from www.codever.land",
          "location": "https://github.com/CodepediaOrg/bookmarks#readme",
          "tags": [
            "programming",
            "resource",
            "blog",
            "open-source"
          ],
          "sourceCodeURL": "https://github.com/CodepediaOrg/bookmarks",
          "likeCount": 0
        },
        {
          "_id": "5e9d4a463b837e57e76de0ac",
          "name": "Bookmarks Manager for Devevelopers & Co",
          "location": "https://www.codever.land/",
          "tags": [
            "programming",
            "blog",
            "resources",
            "open-source"
          ],
          "sourceCodeURL": "https://github.com/CodepediaOrg/bookmarks.dev",
          "likeCount": 0
        },
        {
          "_id": "5e9d4a463b837e57e76de0ab",
          "name": "Share coding knowledge – CodepediaOrg",
          "location": "https://www.codepedia.org/",
          "tags": [
            "programming",
            "blog",
            "open-source"
          ],
          "sourceCodeURL": "",
          "likeCount": 0
        }
      ],
      "feed": [
        {
          "_id": "5fa8db1897519f34ae94f7e2",
          "name": "Build a CRUD functionality with GraphQL and ExpressJS",
          "location": "https://www.codepedia.org/ama/complete-tutorial-crud-graphql-express",
          "tags": [
            "graphql",
            "expressjs",
            "graphql-express",
            "rest",
            "api-design"
          ],
          "sourceCodeURL": "https://github.com/CodepediaOrg/graphql-express-crud-demo",
          "likeCount": null
        },
        {
          "_id": "5f93b3a51e55b52d7b5d73bd",
          "name": "Issues · BookmarksDev/bookmarks.dev · GitHub",
          "location": "https://github.com/codeverland/codever/issues",
          "tags": [
            "bookmarksdev"
          ],
          "sourceCodeURL": "",
          "likeCount": 0
        }
      ],
      "history": [
        {
          "_id": "5f93b3a51e55b52d7b5d73bd",
          "name": "Issues · BookmarksDev/bookmarks.dev · GitHub",
          "location": "https://github.com/codeverland/codever/issues",
          "tags": [
            "bookmarksdev"
          ],
          "sourceCodeURL": "",
          "likeCount": 0
        }
      ]
    }
  }
}
```

## Resolvers
In the **Query** section before you might have noticed the `resolve` method. These are so called **Resolvers** in
the GraphQL terminology. If the schema defines the structure of the GraphQL API, then the resolvers implement the API and
determine the server's _behaviour_.

> Each field in a GraphQL schema is backed by a resolver

" In its most basic form, a GraphQL server will have one resolver function _per field_ in its schema.
 Each resolver knows how to fetch the data for its field. Since a GraphQL query at its essence is just a collection of fields,
  all a GraphQL server actually needs to do in order to gather the requested data is invoke all the resolver functions
   for the fields specified in the query. (This is also why GraphQL often is compared to RPC-style systems,
    as it essentially is a language for invoking remote functions.)"[^3]

[^3]: <https://www.prisma.io/blog/graphql-server-basics-the-schema-ac5e2950214e>

### Anatomy of the resolver

Let's see the code snippet again for the `bookmark` query:
```javascript
        bookmark: {
            type: Bookmark,
            args: {
                bookmarkId: {type: GraphQLID}
            },
            resolve: async (root, args, context, info) => {
                const bearerToken = context.bearerToken;
                const {bookmarkId} = args;
                const response = await bookmarksApiService.getBookmarkById(userId, bearerToken, bookmarkId);

                return response.body;
            }
        }
```

Notice the **parameters** of the `resolve` function. They have the following meaning:

"
1. `root` (also sometimes called parent): Remember how we said all a GraphQL server needs to do to resolve a query
 is calling the resolvers of the query’s fields? Well, it’s doing so breadth-first (level-by-level) and the root argument
  in each resolver call is simply the result of the previous call (initial value is null if not otherwise specified).
2. `args`: This argument carries the parameters for the query, in this case the id of the User to be fetched.
3. `context`: An object that gets passed through the resolver chain that each resolver can write to and read from (basically a means for resolvers to communicate and share information).
4. `info`: An AST representation of the query or mutation. You can read more about the details [Demystifying the info Argument in GraphQL Resolvers](https://blog.graph.cool/graphql-server-basics-demystifying-the-info-argument-in-graphql-resolvers-6f26249f613a).
" [^3]

### Set parameter into the resolver's context in express middleware
You can also set parameters into the `req` object of the Express middleware and they will be available in the
`context` parameter in resolvers, as this is the cases for `bearerToken` from the previous example - `const bearerToken = context.bearerToken;`

```javascript
const app = express();

const setAccessTokenMiddleware = async (req, res, next) => {
  const accessToken = await accessTokenService.getKeycloakAccessToken();
  req.bearerToken = 'Bearer ' + accessToken;

  const decoded = jwt.decode(accessToken);
  const userId = decoded.sub;
  req.userId = userId;
  next();
}

app.use(setAccessTokenMiddleware);
```


The `bearerToken` is set into the context via the Express Middleware

## Mutations - the CUD in CRUD
If queries are used for fetching data from the GraphQL server, then mutations are to modify the data on the GraphQL server.

"In REST, any request might end up causing some side-effects on the server, but by convention it's suggested that one doesn't use GET requests to modify data.
 GraphQL is similar - technically any query could be implemented to cause a data write.
 However, it's useful to establish a convention that any operations that cause writes should be sent explicitly via a mutation.

Just like in queries, if the mutation field returns an object type, you can ask for nested fields.
 This can be useful for fetching the new state of an object after an update. "[^4]

[^4]: <https://graphql.org/learn/queries/#mutations>

Let's see what mutations are available for the demo project:
```
type Mutation {
    createBookmark(input: BookmarkInput!): Bookmark
    updateBookmark(bookmarkId: ID!, input: BookmarkInput!): Bookmark
    deleteBookmark(bookmarkId: ID!): Bookmark
}
```

and the implementation in GraphQL-js is the following:

```javascript
const Mutation = new GraphQLObjectType({
    name: 'Mutation',
    fields: {
        createBookmark: {
            type: Bookmark,
            args: {
                input: {type: BookmarkInput}
            },
            resolve: async (root, args, context) => {
                const { input } = args;

                const {userId, bearerToken} = context;
                const bookmark = await bookmarksApiService.createBookmark(bearerToken, userId, input);

                return bookmark;
            }
        },
        updateBookmark: {
            type: Bookmark,
            args: {
                bookmarkId: {type: GraphQLID},
                input: {type: BookmarkInput}
            },
            resolve: async (root, args, context) => {
                const { input, bookmarkId } = args;

                const {userId, bearerToken} = context;
                const bookmark = await bookmarksApiService.updateBookmark(bearerToken, userId, bookmarkId, input);

                return bookmark;
            }
        },
        deleteBookmark: {
            description: "Given its ID a bookmark can be deleted. Either by the one that created it or an Admin",
            type: Bookmark,
            args: {
                bookmarkId: {type: GraphQLID}
            },
            resolve: async (root, args, context) => {
                const bookmarkId = args.bookmarkId;
                const {userId, bearerToken} = context;
                const deletedBookmark = await bookmarksApiService.deleteBookmarkId(bearerToken, userId, bookmarkId);
                return deletedBookmark;
            }
        }
    }
});
```

## Conclusion
In this post you've learned a bit of theory about GraphQL's main elements accompanied by examples with their corresponding
implementation in GraphQL-JS. I really start to like GraphQL and I say it one more time - the best learning experience is a hands-on one.




