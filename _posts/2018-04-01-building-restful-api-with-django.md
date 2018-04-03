---
layout: post
title: Building RESETful APIs with the Django
tags: [Python, Django, RESETful APIs]
---

## 1. Background and Requirements

### Background

The current RnDGo web application (Version 1) uses the traditional Django method (RPC-based) to provide resources to the front-end users. So far, we can handle the HTTP requests for each module. However, more and more problems to raising:

- The endpoints (URLs) are not systematically organized. The more apps we add in, the messier for routing the HTTP requests.

- Many of the APIs and front-end are heavily coupled. The rendering is also heavy from a view to a template.

- All the Views-URLs-Templates are for mainly for desktop users, they are not well crafted for mobile users. If we are developing mobile apps, developers will suffer.

- No versioning controls over the APIs. If any substantial changes made to the existing APIs, the front-end will break as some of the data wont be rendered. And, an older version app might not work any more (consider about future Mobile Apps or third part plug-ins, they may not updated to the lasted).

### Requirements

To solve the above problem, we need to re-design/re-organize our APIs systematically and strategically using available tools in the market (we do not want to reinvent the wheels). The requirements are listed below:

- One API for all platforms
- Decouple/Loose coupling from front-end
- Django compatible
- Easy to version and maintain
- Open source

## 2. Criteria and Solution

There are two solutions meeting our requirements and available for us: `REST API` using **[Django RESET Framework](http://www.django-rest-framework.org/) (DRF)**, and `GraphQL API` using **[Graphene-Python](http://docs.graphene-python.org/projects/django/en/latest/) (GraphQL)**

For detailed differences between RPC-Based and REST-base HTTP API, please refer to [this article](https://www.smashingmagazine.com/2016/09/understanding-rest-and-rpc-for-http-apis/).

For detailed differences between REST and GraphQL, go to [this article](https://philsturgeon.uk/api/2017/01/24/graphql-vs-rest-overview/)

Those two are not exclusive and can be used at the same time. But due to our limited resources, we need to pick one to start. To evaluate them, there are a few criteria to be met:

- (1) The wildly used the better
- (2) The easier to adapt and debug the better
- (3) No breaking changes to our current system
- (4) Well documented tutorial with active community supports
- (5) Trend

For each of the criteria, a 3-level scoring system is employed to evaluate each of them: `0` for `Not satisfied`; `1` for `Somewhat satisfied`; and `3` for `Completely satisfied`.

To compare them:

| Criteria | (1) | (2) | (3) | (4) | (5) |
| -------- | --- | --- | --- | --- | --- |
| DRF      | 2   | 2   | 2   | 2   | 1   |
| GraphQL  | 1   | 1   | 1   | 2   | 2   |

To sum up, **DRF** stands out overall, due to its similar logic to our current methods (`serializers` in RDF is very similar to the `forms` we are using), very mature and wildly used, and easier to follow.

## 3. General API Design Guidance (TLTR;)

There are some basic concepts to be kept in mind when using DRF:

- API Endpoint(s) [Server-side URLs]

  - Retrieve, Update, Delete
  - Create & List & Search

- HTTP methods [Client-side methods]

  - GET, POST, PUT, PATCH, DELETE

- Data Types & Validation
  - JSON -> Serializer
  - Validation -> Serializer

The following RESTful API design best practices are purely got from [Philipp Hauer's Blog](https://blog.philipphauer.de/restful-api-design-best-practices/), and we are following them. All credits go to Philipp Hauer. You can also refer to [How to design a RESTful API architecture from a human-language spec](https://www.oreilly.com/learning/how-to-design-a-restful-api-architecture-from-a-human-language-spec) for even more detail steps on design RESTful API.

### Use Two URLs per Resource

One URL for the collection and one for a single resource:

```
# URL that represents a collection of resources
/employees
# URL that represents a single resource
/employees/56
```

### Use Consistently Plural Nouns

Prefer

```
/employees
/employees/21
```

over

```
/employee
/employee/21
```

### Use Nouns instead of Verbs for Resources

This will keep you API simple and the number of URLs low. Don’t do this:

```
/getAllEmployees
/getAllExternalEmployees
/createEmployee
/updateEmployee
```

### Use HTTP Methods to Operate on your Resources

```
GET /employees
GET /employees?state=external
POST /employees
PUT /employees/56
```

Use URLs to specify _the resources_ you want to work with. Use the HTTP methods to specify _what_ to do with this resource. With the five HTTP methods GET, POST, PUT, PATCH and DELETE you can provide CRUD functionality (Create, Read, Update, Delete) and beyond.

- **Read**: Use GET for reading resources.
- **Create**: Use POST or PUT for creating new resources.
- **Update**: Use PUT and PATCH for updating existing resources.
- **Delete**: Use DELETE for deleting existing resources.

### Wrap the Actual Data in a data Field

`GET /employees` returns a list of objects in the data field:

```json
{
  "data": [{ "id": 1, "name": "Larry" }, { "id": 2, "name": "Peter" }]
}
```

`GET /employees/1` returns a single object in the data field:

```json
{
  "data": {
    "id": 1,
    "name": "Larry"
  }
}
```

The payload of PUT, POST and PATCH requests should also contain the data field with the actual object.

Advantages:

- There is space left to add metadata (e.g. for pagination, links, deprecation warnings, error messages)
- Consistency
- Compatible with the [JSON: API Standard](http://jsonapi.org/)

### Use the Query String (`?`) for Optional and Complex Parameters

Don’t do this:

```
GET /employees
GET /externalEmployees
GET /internalEmployees
GET /internalAndSeniorEmployees
```

Keep your URLs simple and the URL set small. Choose one base URL for your resource and stick to it. Move complexity or optional parameters to the query string.

```
GET /employees?state=internal&title=senior
GET /employees?id=1,2
```

The JSON:API way of filtering is:

```
GET /employees?filter[state]=internal&filter[title]=senior
GET /employees?filter[id]=1,2
```

### Use HTTP Status Codes

The RESTful Web Service should respond to a client’s request with a suitable HTTP status response code.

- **2xx** – success – everything worked fine.
- **4xx** – client error – if the client did something wrong (e.g. the client sends an invalid request or he is not authorized)
- **5xx** – server error – failures on the server-side (errors while trying to process the request like database failures, dependend services are not available, programming errors or states that should not occur)

Consider the available HTTP status codes. However, be aware, that using all of them could be confusing for the users of your API. Keep the set of used HTTP status codes small. It’s common to use the following codes:

- **2xx: Success**
  - 200 OK
  - 201 Created
- **3xx: Redirect**
  - 301 Moved Permanently
  - 304 Not Modified
- **4xx: Client Error**
  - 400 Bad Request
  - 401 Unauthorized
  - 403 Forbidden
  - 404 Not Found
  - 410 Gone
- **5xx: Server Error**
  - 500 Internal Server Error

**Don’t overuse 404**. Try to be more precise. If the resource is available, but the user is not allowed to view it, return a 403 Forbidden. If the resource existed once but now has been deleted or deactivated, use 410 Gone.

### Provide Useful Error Messages

Additionally to an appropriate status code, you should provide a useful and verbose description of the error in the body of your HTTP response. Here’s an example.

Request:

```
GET /employees?state=super
```

Response:

```json
// 400 Bad Request
{
  "errors": [
    {
      "status": 400,
      "detail": "Invalid state. Valid values are 'internal' or 'external'",
      "code": 352,
      "links": {
        "about": "http://www.domain.com/rest/errorcode/352"
      }
    }
  ]
}
```

### Design Relationships Appropriately

Let’s assume that each `employee` has a `manager` and several `teamMembers`. There are basically three common options to design relationships within an API: Links, Sideloading and Embedding.

They are all valid and the right choice depends on the use case. Basically, you should design the relationships **depending on the client’s access schema and the tolerable request amount and payload size**.

#### Links

```json
{
  "data": [
    {
      "id": 1,
      "name": "Larry",
      "relationships": {
        "manager": "http://www.domain.com/employees/1/manager",
        "teamMembers": [
          "http://www.domain.com/employees/12",
          "http://www.domain.com/employees/13"
        ]
        //or "teamMembers": "http://www.domain.com/employees/1/teamMembers"
      }
    }
  ]
}
```

- Small payload size. It’s good, if the client doesn’t need the `manager` and the `teamManager` every time.
- Many Requests. It’s bad, if nearly every client needs this data. Many additional requests may be required; in the worse case for every employee. And this is multiplied by every relationship (`manager`, `teamMembers` and so on) an employee has.
- The client has to stitch the data together in order to get the big picture.

#### Sideloading

We can refer to the relationship with a foreign key and put the referred entities also in the payload but under the dedicated field `included`. This approach also called “Compound Documents”.

```json
{
  "data": [
    {
      "id": 1,
      "name": "Larry",
      "relationships": {
        "manager": 5,
        "teamMembers": [12, 13]
      }
    }
  ],
  "included": {
    "manager": {
      "id": 5,
      "name": "Kevin"
    },
    "teamMembers": [{ "id": 12, "name": "Albert" }, { "id": 13, "name": "Tom" }]
  }
}
```

The client may also control the sideloaded entities by a query parameter like `GET /employees?include=manager,teamMembers`.

- We get along with a single request.
- Tailored payload size. No duplication (e.g. you only deliver a manager once even if he is referenced by many employees)
- The client still has to stitch the data together in order to resolve the relationships, which can be very cumbersome.

#### Embedding

```json
{
  "data": [
    {
      "id": 1,
      "name": "Larry",
      "manager": {
        "id": 5,
        "name": "Kevin"
      },
      "teamMembers": [
        { "id": 12, "name": "Albert" },
        { "id": 13, "name": "Tom" }
      ]
    }
  ]
}
```

- Most convenient for the client. Is can directly follow the relationships to get the actual data.
- Relationships may be loaded in vain if the client doesn’t need it.
- Increased payload size and duplications. Referenced entities may be embedded multiple times.

### Use CamelCase for Attribute Names

Use CamelCase for your attributes identifiers.

```json
{ "yearOfBirth": 1982 }
```

Don’t use underscores (`year_of_birth`) or capitalize (`YearOfBirth`). Often your RESTful web service will be consumed by a client written in JavaScript. Typically the client will convert the JSON response to a JavaScript object (by calling `var person = JSON.parse(response) )` and call its attributes. Therefore, it’s a good idea to stick to the JavaScript convention which makes the JavaScript code more readable and intuitive.

```js
// Don't
person.year_of_birth; // violates JavaScript convention
person.YearOfBirth; // suggests constructor method

// Do
person.yearOfBirth;
```

### Use Verbs for Operations

Sometimes a response to an API call doesn’t involve resources (like calculate, translate or convert). Example:

```bash
# Reading
GET /translate?from=de_DE&to=en_US&text=Hallo
GET /calculate?para2=23&para2=432

# Trigger an operation that changes the server-side state
POST /restartServer
# no body

POST /banUserFromChannel
{ "user": "123", "channel": "serious-chat-channel" }
```

### Provide Pagination

It is almost never a good idea to return all resources of your database at once. Consequently, you should provide a pagination mechanism. Two popular approaches are:

- Offset-based Pagination
- Keyset-based Pagination aka Continuation Token aka Cursor (recommended)

#### Offset-based Pagination

A really simple approach is to use the parameters offset and limit, which are well-known from databases.

```bash
/employees?offset=30&limit=15 # returns the employees 30 to 45
```

If the client omits the parameter you should use defaults (like `offset=0` and `limit=100`). Never return all resources. If the retrieval is more expensive you should decrease the limit.

```
/employees # returns the employees 0 to 100
```

You can provide links for getting the next or previous page. Just construct URLs with the appropriate offset and limit.

```
GET /employees?offset=20&limit=10
```

```json
{
  "pagination": {
    "offset": 20,
    "limit": 10,
    "total": 3465
  },
  "data": [
    //...
  ],
  "links": {
    "next": "http://www.domain.com/employees?offset=30&limit=10",
    "prev": "http://www.domain.com/employees?offset=10&limit=10"
  }
}
```

#### Keyset-based Pagination (aka Continuation Token, Cursor)

The presented offset-based pagination is easy to implement but comes with severe drawbacks. They are slow (SQL’s `OFFSET` clause becomes very slow for large numbers) and unsafe (it’s easy to miss elements when changes are happening during pagination).

That’s why it’s better to use an indexed column. Let’s assume that our employees have an indexed column `data_created` and the collection resource `/employees?pageSize=100` returns the oldest 100 employees sorted by this column. Now, the client only has to take the `dateCreated` timestamp of the last employee and uses the query parameter `createdSince` to continue at this point.

```bash
GET /employees?pageSize=100
# The client receives the oldest 100 employees sorted by `data_created`
# The last employee of the page has the `dataCreated` field with 1504224000000 (= Sep 1, 2017 12:00:00 AM)

GET /employees?pageSize=100&createdSince=1504224000000
# The client receives the next 100 employees since 1504224000000.
# The last employee of the page was created on 1506816000000. And so on.
```

This solves already many of the disadvantages of offset-based pagination, but it’s still not perfect and not very convenient for the client.

- It’s better to create a so-called continuation token by adding additional information (like the id) to the date in order to improve the reliability and efficiency.
- Moreover, you should provide a dedicated field in the payload for that token so the client doesn’t have to figure it out by looking at the elements. You can even go further and provide a next link.

So `GET /employees?pageSize=100` returns:

```json
{
  "pagination": {
    "continuationToken": "1504224000000_10"
  },
  "data": [
    // ...
    // last element:
    { "id": 10, "dateCreated": 1504224000000 }
  ],
  "links": {
    "next":
      "http://www.domain.com/employees?pageSize=100&continue=1504224000000_10"
  }
}
```

The next link makes the API really RESTful as the client can page through the collection simply by following these links (HATEOAS). No need to construct URLs manually. Moreover, you can simply change the URL structure without breaking clients (evolvability).

### Ensure Evolvability of the API

#### Avoid Breaking Changes

- **Ideally, REST APIs (as every API) should be stable**. Basically, breaking changes (like changing the whole payload format or the URL scheme) should not happen. But how can we still evolve our API without breaking the clients?

- **Make backward-compatible changes**. Adding fields is no problem (as long as the clients are tolerant).
- Duplication and Deprecation. In order to change an existing field (rename or change structure), you can add the new one next to the old field and deprecated the old one in the documentation. After a while, you can remove the old field.

- **Utilize Hypermedia and HATEOAS**. As long as the API client uses the links in the response to navigate through the API (and doesn’t craft the URLs manually), you can safely change the URLs without breaking the clients.
- **Create new resources with new names**. If new business requirements lead to a completely new domain model and workflows, you can create new resources. That’s often quite intuitive as the domain model has a new name anyway (derived from the business name). Example: A rental service now also rents bikes and segways. So the old concept car with the resource `/cars` doesn’t cut it anymore. A new domain model vehicle with a new resource `/vehicles` is introduced. It’s provided along with the old `/cars` resource.

#### Consider API Versioning

You should at least contemplate a versioning approach for your API. It allows you to release incompatible and breaking changes of your API under a new version without breaking the clients. They can continue consuming the old version. The clients can migrate to the new version at their own speed.

This topic is hotly disputed in the community and there are many different approaches for versioning. But two of them are very popular:

```
Versioning via URLs: /v1/
Versioning via the Accept HTTP Header: Accept: application/vnd.myapi.v1+json (Content Negotiation)
```

#### Versioning via URLs

Just put the version number of your API in the URL of every resource.

```
/v1/employees
```

Pros:

- Extremely simple for API developers.
- Extremely simple for API clients.
- URLs can be copied and pasted.

Cons:

- Not RESTful.
- Breaking URLs. Clients have to maintain and update the

Due to its simplicity, URL versioning is very popular and widely used by companies like Facebook, Twitter, Google/YouTube, Bing, Dropbox, Tumblr, and Disqus. Now many of them are moving to `GraphQL`, which make APIs more user-friendly and secure.

## 4. Summary

If you have the resources (time, money and human power), learn and choose GraphQL. If you have legacy APIs to maintain and update, choose DRF for the time being and slowly integrate GraphQL into your system. Nevertheless, the basic API design principles stays the same.
