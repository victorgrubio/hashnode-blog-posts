## API Documentation with OpenAPI

Your backend needs an API to expose all its value to the world. You already know that. And it should be well commented to avoid any user misusing or hate to use it.

An API can be considered as the contract between the **information provider** (a lovely *backend developer*, like me 😇) and the **consumer** (an *awful frontend* developer, like me 😈). There, the content required for the producer (**request**) to provide the information that the consumer needs (**response**) is defined.

There we hide all of our terrible backend logic from our users and give them a nice, simple way to use our services. And, if it is in exchange for money, better 💸💸💸

![backend-frontend-meme.png](https://i.pinimg.com/originals/5b/d6/8c/5bd68c6d04eb8a54e0bab9380b46db3b.png)

# OpenAPI

> Okay Victor, I know a little bit about APIs, maybe I even know how to create one ... BUT HOW THE F DO I DOCUMENT IT?

Nice question. Tons of people did the same one. Some of them united and created the  [OpenAPI Specification (OAS)](https://swagger.io/docs/specification/about/).

The OpenAPI Specification defines a standard, language-agnostic interface to RESTful which permits both people and computers to find and get the value from the services without getting to source code or documentation.  If defined properly, users can understand and interact with the remote service with a low effort from their site. I mean, at least they need access to the Internet 😅😅

An OpenAPI file allows you to describe your entire API, including:

- Available endpoints (/ideas) and methods on each endpoint (GET /ideas, POST /ideas)
- Operation parameters Input and output for each method
- Authentication methods
- Contact information, license, terms of use, and other information.

---
# Documentation-First vs Code-First Workflow

> To *code first* or  to *document first*, that is  the question

I have created quite a few APIs, most of them using API documentation. Some developers like to create the OpenAPI file **from the development**. Most programming languages and frameworks have a plugin to generate OpenAPI documentation from your code. 

A list for some of the most used:

- [Flask](https://github.com/flasgger/flasgger)
- [Django](https://django-rest-swagger.readthedocs.io/en/latest/)
- [Spring Boot](https://springdoc.org/#Introduction)
- [NodeJS](https://www.npmjs.com/package/swagger-ui-express)


There is another way to view it. Automatically creating your code **from the documentation**.

As OpenAPI doesn't care about your programming language, it is able to generate code from the YAML/JSON file to the framework you specify. Then, you just need to create the server logic and your API is ready to go!

Some useful tools that allow you to generate code from the documentation:

- [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator) - Open Source Tool to create client/server versions of our API
- [Swagger Online Editor](https://editor.swagger.io/) - Edit & Lint your OpenAPI files
- [SwaggerHub](https://swagger.io/tools/swaggerhub/) - Design & Publish your documentation
- [Swagger Codegen](https://swagger.io/tools/swagger-codegen/) - A Swagger tool to generate client libraries for your API in over 40 languages and generate a server stub for your API.

![AvailableOpenAPI.png](https://static1.smartbear.co/swagger/media/images/tools/opensource/swagger_codegen.png?ext=.png)

---
# Create an OpenAPI documentation file
> Let's make an example to learn

### Idea API

To illustrate most of the concepts without creating a difficult logic, let's create an Idea API.

This will be a simple ** Create Read Update Delete (CRUD) ** API, with just the Idea as a model (no users or anything like that)

## Definition & Concepts

First, go to [Swagger Online Editor]. It will show the classic PetStore Example using Swagger 2.0. We will like to convert this example to OpenAPI v3. 

To do so, click on ** Edit -> Convert to OpenAPI 3**

Now let's delete most of it, leaving only the top part, without any paths:

```
openapi: 3.0.1
info:
  title: Swagger Petstore
  description: 'This is a sample server Petstore server.  You can find out more about     Swagger
    at [http://swagger.io](http://swagger.io) or on [irc.freenode.net, #swagger](http://swagger.io/irc/).      For
    this sample, you can use the api key `special-key` to test the authorization     filters.'
  termsOfService: http://swagger.io/terms/
  contact:
    email: apiteam@swagger.io
  license:
    name: Apache 2.0
    url: http://www.apache.org/licenses/LICENSE-2.0.html
  version: 1.0.0
externalDocs:
  description: Find out more about Swagger
  url: http://swagger.io
servers:
- url: https://petstore.swagger.io/v2
- url: http://petstore.swagger.io/v2
tags:
- name: pet
  description: Everything about your Pets
  externalDocs:
    description: Find out more
    url: http://swagger.io
- name: store
  description: Access to Petstore orders
- name: user
  description: Operations about user
  externalDocs:
    description: Find out more about our store
    url: http://swagger.io
paths:
components:
security:
```

Let me describe all the keys used in the previous piece of documentation

| Property     | Description                                                     | Type                     | Value                                                                                                                                                                                       | More Info                                                     |
|--------------|-----------------------------------------------------------------|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| openapi      | Version of OpenAPI you are using                                | Version String           | 3.0.1                                                                                                                                                                                       | https://swagger.io/docs/specification/basic-structure/        |
| info         | Description of your API                                         | Object                   | termsOfService: http://swagger.io/terms/   contact:     email: apiteam@swagger.io   license:     name: Apache 2.0     url: http://www.apache.org/licenses/LICENSE-2.0.html   version: 1.0.0 | https://swagger.io/docs/specification/basic-structure/        |
| servers      | List of servers to make requests to                             | List of urls             | - url: https://petstore.swagger.io/v2 - url: http://petstore.swagger.io/v2                                                                                                                  | https://swagger.io/docs/specification/api-host-and-base-path/ |
| tags         | List of groups where endpoints can be joined                    | List                     | - name: pet   description: Everything about your Pets   externalDocs:     description: Find out more     url: http://swagger.io                                                             | https://swagger.io/docs/specification/basic-structure/        |
| paths        | List of endpoints available for the API, along with the methods | Object                   | /ideas:    - get:        description: List all ideas                                                                                                                                        | https://swagger.io/docs/specification/paths-and-operations/   |
| parameters   | List of parameters using in the API                             | Object                   | parameters:  Id:    description: Id of the Idea    type: integer                                                                                                                            | https://swagger.io/docs/specification/describing-parameters/  |
| components   | List of Schemas available in the API                            | Object                   | components   Idea:     description: Idea model     required:      - id     properties:       id:          description: id of object         type: integer                                   | https://swagger.io/docs/specification/components/             |
| security     | Security / Authentication of the API                            | Object                   | security:   - ApiKeyAuth: [] - OAuth2:    - read      - write                                                                                                                               | https://swagger.io/docs/specification/authentication/         |
| externalDocs | Url to external documentation of the API                        | Object (description/url) | externalDocs:  name: Example externals  url: http://swagger.io                                                                                                                              | https://swagger.io/docs/specification/basic-structure/        |

As mentioned before, you have a detailed explanation in the [OpenAPI Documentation](https://swagger.io/docs/specification/about/)



You will see alerts in the editor. No worries, we have to define our methods yet.

These methods are the services we allow the users to interact with. As some of you may know the most common HTTP methods are:

- **`GET` **-> Retrieves information
- **`POST` **-> Send loads of data to a server to create or update a resource
- **`PUT` **-> Complete update of an object
- **`PATCH` **-> Partial update of an object
- **`DELETE` **-> Remove object

Other additional methods, less common but also important:

- **`HEAD` **-> Equal to `GET`, without receiving response items (Check that GET works)
- **`OPTIONS` ** ->  Returns data describing what other methods and operations the server supports at the given URL

A detailed article about HTTP methods is available [here](https://assertible.com/blog/7-http-methods-every-web-developer-should-know-and-how-to-test-them), as it is out of the scope of this one.

### Endpoint Creation & Parameters

As mentioned before, we are going to use GET, POST, PUT and DELETE methods for our API.

Let's create the List and Create endpoints:

```
openapi: 3.0.1
info:
  title: Idea API
  description: 'This is a sample server for the Idea API from the blog post "API Documentation with OpenAPI" by @victorgarciadev'
  termsOfService: http://swagger.io/terms/
  contact:
    email: victorgrubiodl@gmail.com 
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT
  version: 1.0.0
externalDocs:
  description: Blog with amazing articles
  url: https://www.victorgarciar.com
servers:
- url: https://petstore.swagger.io/v2
tags:
- name: ideas
  description: Everything about your Ideas
paths:
  /ideas:
    get:
      summary: List all ideas
      description: Retrieves a list of all the ideas available
      tags:
       - ideas
      responses:
       "200":
          description: List of ideas
          content:
            application/json:
              schema:
                type: array
                items:
                 $ref: '#/components/schemas/Idea'
       "500":
          description: Internal Server Error
          content:
            application/json:
              schema:
                type: object
                properties:
                 message: 
                   type: string
                   description: Error message
                   example: 'This is an error message for Internal Server Error'
    post:
      summary: Create idea
      description: Creates a new idea from its basic data
      tags:
       - ideas
      requestBody:
        description: Data from idea that will be created
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Idea'
            example:
              description: Idea to create
      responses:
       "200":
          description: The created idea
          content:
            application/json:
              schema:
               $ref: '#/components/schemas/Idea'
       "500":
          description: Internal Server Error
          content:
            application/json:
              schema:
                type: object
                properties:
                 message: 
                   type: string
                   description: Error message
                   example: 'This is an error message for Internal Server Error'
components:
  schemas:
    Idea:
      type: object
      description: Idea
      properties:
       id: 
        type: integer
        description: id of item
        minimum: 1
       description:
        type: string
        description: Description of idea
```

And that should render to something like this


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632761526640/8lrXcTCmI.png)
NOTE: You can observe that we have refactored the metadata of our API to be adjusted to this project.

Now let's create the Read, Update and Delete endpoints. Below the definition of the */ideas* path, add the following code:

```
/ideas/{ideaId}:
    get:
      summary: List all ideas
      description: Retrieves a list of all the ideas available
      tags:
       - ideas
      parameters:
        - name: ideaId
          in: path
          required: true
          schema:
            type: integer
            format: int64
      responses:
       "200":
          description: Idea requested
          content:
            application/json:
              schema:
                 $ref: '#/components/schemas/Idea'
       "404":
          $ref: '#/components/responses/404NotFound'
       "500":
          description: Internal Server Error
          content:
            application/json:
              schema:
                type: object
                properties:
                 message: 
                   type: string
                   description: Error message
                   example: 'This is an error message for Internal Server Error'
    put:
      summary: Updates an idea
      description: Performs an update of an specific idea by the data provided
      tags:
       - ideas
      parameters:
        - name: ideaId
          in: path
          required: true
          schema:
            type: integer
            format: int64
      requestBody:
        description: Data to update the idea
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Idea'
            example:
              description: Idea to update
      responses:
       "200":
          description: Updated idea
          content:
            application/json:
              schema:
                 $ref: '#/components/schemas/Idea'
       "404":
          $ref: '#/components/responses/404NotFound'
       "500":
          description: Internal Server Error
          content:
            application/json:
              schema:
                type: object
                properties:
                 message: 
                   type: string
                   description: Error message
                   example: 'This is an error message for Internal Server Error'
    delete:
      summary: Deletes an idea
      description: Removes an idea if exists
      tags:
       - ideas
      parameters:
        - name: ideaId
          in: path
          required: true
          schema:
            type: integer
            format: int64
      responses:
       "200":
          description: Empty body for ok
          content:
            application/json:
              schema:
                type: object
       "404":
          $ref: '#/components/responses/404NotFound'
       "500":
          description: Internal Server Error
          content:
            application/json:
              schema:
                type: object
                properties:
                 message: 
                   type: string
                   description: Error message
                   example: 'This is an error message for Internal Server Error'
```

Here we can see that we have included a parameter. They can be either specified in the same method (*Read Method*) or we can create a reference to them in the **Parameters** section of the documentation (*Update Method*)

### Model Definition & Object Reference

As you may observe, request bodies and responses include a reference to an Idea object. In the schemas' part, we define all the objects we may use for our bodies and responses. This will create a much more organized structure and reduce duplicities.

A Schema contains the following properties:

- Summary -> Brief description of the object
- Description -> Complete information about the model
- Required -> List of required properties for the model
- Properties -> List of attributes of the model
- Example -> Object used to complete the documentation as default. Could be one or more. [Documentation](https://swagger.io/docs/specification/adding-examples/)

A more detailed explanation about attribute types and object creation can be found in the [OpenAPI Section](https://swagger.io/docs/specification/data-models/)

An example would be the Idea model

```
    Idea:
      type: object
      description: Idea
      properties:
       id: 
        type: integer
        description: id of item
        minimum: 1
       description:
        type: string
        description: Description of idea
```

We can reference it in the documentation with the following code:

```
              schema:
                 $ref: '#/components/schemas/Idea'
```

This applies also to responses and parameters:

```
   delete:
      summary: Deletes an idea
      description: Removes an idea if exists
      tags:
       - ideas
      parameters:
        - name: ideaId
          in: path
          required: true
          schema:
            type: integer
            format: int64
      responses:
       "200":
          description: Empty body for ok
          content:
            application/json:
              schema:
                type: object
       "404":
          $ref: '#/components/responses/404NotFound'


  responses:
    404NotFound:
      description: Not Found
      content:
        application/json:
          schema:
            type: object
            properties:
             message: 
               type: string
               description: Error message
     
          example: 'Idea with id requested not found in server'
```

More info about referencing [here](https://swagger.io/docs/specification/using-ref/)

### Security

We are going to add a simple API Key security layer for the sake of completeness. To do so, we are going to define a `Security Schemes` property at the root of the document

The security schema may vary in type depending on your project's conditions. We can define the security in a more granular way by specifying it on each method.

We have to add the API Key auth to the `securitySchemes` property at *Components*. Then add the `security` property, at the same level of paths, components, etc.

```
  securitySchemes:
    apiKey:
      type: apiKey
      in: header
      name: X-API-Key
security:
  - apiKey: []
```

More info about Security Schemes in OpenAPI [here](https://swagger.io/docs/specification/authentication/).

---
# Share it online with SwaggerHub
> Someone may use your API one day. Go on and share it!

Now that we have our API ready, let's share it with everyone. As you may know, if you follow my *Daily Dev Tips* from [Twitter](https://twitter.com/VictorGarciaDev), I like a lot Swaggerhub to define and expose our API documentation.

Let's create a new Project there, checking `Import and document API`

![create-swaggerhub.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632763294413/1mrW29z40.png)

Then, we can download our OpenAPI documentation file from Swagger Editor and add it to the project

![export-api-editor.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632763319446/hAvHf6fVI.png)

And it's done, we have our API documentation published for everyone to see. [Here's mine](https://app.swaggerhub.com/apis-docs/victorgarciar/idea-api/1.0.0)


![swaggerhub-loaded.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632763508289/j-bY50G8E.png)

---
# Summary

We have (I hope) learned in this article:

- What's an API
- Why is important to document your APIs with OpenAPI
- How an OpenAPI documentation file looks like
- Implement OpenAPI documentation for a CRUD API
- Share it using Swaggerhub

![patrick-fun-gif](https://media.giphy.com/media/8WJw9kAG3wonu/giphy.gif?cid=ecf05e476rq397rl82lfotvyar9xwkkrl5wgzrk8p9mr1akx&rid=giphy.gif)

I am HONOURED if you have reached this point. Thanks a lot for reading 💙

Hopefully, I was able to explain the importance of API documentation and how to do it using OpenAPI.

Reach out on [Twitter](https://twitter.com/VictorGarciaDev) to find more valuable content or just chatting!

🐦 [@victorgarciadev](ttps://twitter.com/VictorGarciaDev)

😼 https://github.com/victorgrubio

## References

- [OpenAPI Documentation](https://swagger.io/docs/specification/about/)
- [HTTP Methods](https://assertible.com/blog/7-http-methods-every-web-developer-should-know-and-how-to-test-them)


Thanks and Keep It Up! 🦾🦾