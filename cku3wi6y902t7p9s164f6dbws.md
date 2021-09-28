## How to create a Postman Collection from OpenAPI Docs

In the [previous article of this series](https://www.victorgarciar.com/api-documentation-with-openapi), we learned how to create the documentation for your API using OpenAPI.

If you didn't check it out,  what are you waiting for !?!? 😁😁

Today we will see in this brief tutorial how to create a [Postman](https://www.postman.com) collection from this documentation.

---
# Download API docs

> We have different ways to download the YAML file containing the documentation. It depends on how we stored it. 

### Swagger Editor

Swagger Editor stores the status of your last edition. Yet, if you have to delete information from your browser, all your progress may be lost! 🆘🆘

Swagger Editor is an excellent tool for fast edition and linting your files. However, if the documentation is large and will take you days, then use editors like Notepad++, Gedit, or VSCode to write it.

To download the API documentation file from Swagger Editor, we have to click on: `Edit`-> `Save as YAML`

![Save-as-yaml-swagger-editor.png](https://i.imgur.com/UtPeRW1.png)

The browser will download an `openapi.yaml` file.

### SwaggerHub

Swagger Hub stores the information on their servers. There is nothing to worry about here 🤗 To download it, we have to click on our API project.

Once we are editing our project, we have to click on `Export` -> `Download API` -> `YAML Resolved`.

![Swaggerhub-export-process.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632820916125/6O3lg2uvh.png)

I think this is the best format to do so. JSON will be valid as well, although I find YAML easier to edit.

---
# Import it to Postman

Now, go on and open Postman. If you don't have it, you can download it from [this link](https://www.postman.com/downloads/). 

We are going to select the `APIs` tab.

![Postman-api-section.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632820815169/esC3Wjjrw.png)

Then let's click on `Import` and select the OpenAPI docs file. Confirm that you want the Collection to act as `Documentation`


![Collection-generation-step.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632819244523/29-Ygx4ln.png)


If you check the APIs section, the definition of your OpenAPI Documentation should appear.


![API-definition-postman.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632819312132/Iq4FyFRGE.png)


Moreover, the generated collection will appear in the `Collections` tab. You should see something like this:


![Postman-collection.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632819332607/F9MHoaXf-.png)


# Create a Mock Server in Swaggerhub

> Do you have a server to make requests to? NO? LET'S MOCK IT THEN! 

Before testing our API, we have to create a *Mock Server* to make requests to! To do so, we are going to take advantage of the *Integrations* available at SwaggerHub.

Click on your API name inside the editor and delete `Integrations` tab.

![Integrations.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632820629074/PVe6HwLJV.png)

Then, add new integration and select `API Auto Mocking`

![AutoMocking-selection.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632820597708/s6Sp1neBX.png)

Set your API name to whatever you want. In this case, I defined `Idea API`.

![Form-with-api-name.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632820648785/ddgWp2h-N.png)

Now the integration is completed. We have our mock server up and running! The address has the following format:

`https://virtserver.swaggerhub.com/<owner>/<api-name>/<version>`

In my case, this results in `https://virtserver.swaggerhub.com/victorgarciar/idea-api/1.0.0`

---
# Test!
> You can't say "It works!" if you haven't tested it yourself

Let's go back to our Postman Collection and set the "baseUrl" variable to the URL of our mock server.

Click on the Collection's name and select the `Variables` tab. You will have a variable there named "baseUrl" as mentioned. Delete the current value and paste your mock server URL!

![Postman-variables.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632820689933/ZNKmeNrYp.png)

Now we have everything set up! Go to one of the requests and send it. You should see something like this:

![Postman-request.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632820727833/Hx28X-NLL.png)

Of course, results will make no sense since we are using a mock server. Don't forget!

---
# Summary

In this article, I hope you have learned:

- Downloading your API docs
- Importing it to Postman
- Creating a mock server in SwaggerHub
- Test your API with a mock server


![patrick-fun-gif](https://media.giphy.com/media/8WJw9kAG3wonu/giphy.gif?cid=ecf05e476rq397rl82lfotvyar9xwkkrl5wgzrk8p9mr1akx&rid=giphy.gif)

I am HONOURED if you have reached this point. Thanks a lot for reading 💙

Hopefully, I was able to explain how to import OpenAPI docs to Postman and create a Mock Server in SwaggerHub to test it!

Reach out on [Twitter](https://twitter.com/VictorGarciaDev) to find more valuable content or just chatting!

🐦 [@victorgarciadev](ttps://twitter.com/VictorGarciaDev)

😼 https://github.com/victorgrubio

## References

- [Previous Article of the Series](https://www.victorgarciar.com/api-documentation-with-openapi)
- [OpenAPI Documentation](https://swagger.io/docs/specification/about/)
- [SwaggerHub Integrations](https://support.smartbear.com/swaggerhub/docs/integrations/api-auto-mocking.html)


Thanks and Keep It Up! 🦾🦾