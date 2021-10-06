## Jmeter API Tests from Postman Collection using Loadium

In the [previous article of this series](https://www.victorgarciar.com/how-to-create-a-postman-collection-from-openapi-docs), we learned how to create a Postman collection from our OpenAPI documentation.

[Check out the complete series](https://www.victorgarciar.com/series/api-design) if you missed it!

Today we will see in this brief tutorial how to create a Jmeter test suite from our collection using Loadium

# Import the collection to Loadium

Our test suite will be generated from the Postman collection in the previous chapter. We have all the CRUD functionality of our Idea API if you remember.

Let's export our collection into JSON. 

- `Right click` on the collection
- Select `Export`
- Select `Collection v2.1 (recommended)`
- Click again on `Export`


![ExportPostmanCollection.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511478000/vUkoZOlWY.png)

Now, we have to open a browser and go [here](https://loadium.com/postman-to-jmeter-converter). You could also go to the [main Loadium site](https://loadium.com). There, select the `Features` tab, and click on the `Convert  Postman to Jmeter`section.


![ConvertPostmanToJMX.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511518193/hODVPAHnlr.png)


It will require you to log in. You could use Google, Github, or Linkedin for doing so, so no worries! After the login process, the home section will appear. Select `Convert to .JMX` option, the last one on the left.


![OptionsLoadium.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511759431/uyk4NoLON.png)

When the form pop-up appears, select the default option `POSTMAN`. Then drag your exported collection file to the drop zone. Now your file will be successfully generated. Click in `Download JMX`. 

![Step1Loadium.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511586935/knPJEnAjS.png)

![Step2Loadium.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511606780/kf_nidirS.png)

# Install Jmeter

To install Jmeter, you should follow these steps provided by [this Blazemeter article](https://www.blazemeter.com/blog/how-get-started-jmeter-installation-test-plans)

[Download Releases](https://jmeter.apache.org/download_jmeter.cgi)

- Install the most recent 64-bit Java Runtime Environment (JRE) or Java Development Kit (JDK). Because JMeter is a pure Java program, this is critical.

- Go to the Apache JMeter website and look for the Binary to save to your PC.

- Move this file to your desired location after downloading it. Unpack it, then go to the folder, then the bin directory.

- Take a peek around. A set of scripts should appear that can run JMeter in various modes.

For me, I have to run on my Ubuntu terminal

```bash
./jmeter
```

and the GUI will appear before my 👀👀

> Don't be afraid of the installation process. Is quite easy although it is not very frequent

![GUIJmeter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511637929/k7o2F9G9G.png)

# Load our JMX and Refactor

> Now we are only a few steps from the glory! 🥳🥳 

Let's add our JMX file by selecting `File` -> `Open`  and navigate to the location of your downloaded JMX file.

Now you should see the testing suite loaded in JMeter


![LoadedJmeter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511669357/mRrTLCUvM.png)

We have to make some refactoring to make it work. If you click on one of the requests, you would see something like this:

![requestBeforeRefactor.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633511682096/ZiU7z8jU9.png)

We have to make the following changes:

- Split the baseUrl into Protocol - Host - Port
- Refactor the :ideaId parameter into JMX Syntax

### Defining default HTTP request Options

First, we will add some basic variables for configuration. To include a config element, let's right click on the engine wheel and select `Add` -> `Config Element` -> `User Defined Variables`

We will include two elements:

- defaultPath: the prefix path  for requests -> `/victorgarciar/idea-api/1.0.0`
- server: the domain of our mock server -> `virtserver.swaggerhub.com`

We want to configure the HTTP server once and inherit it on each request. To do so, we have to create an *HTTP Request Defaults* element by right-clicking on the engine wheel and selecting `Add` -> `Config Element` -> `HTTP Defaults`. 

We have to add the server variable. Left the port empty as we are using the 89. It results as follows:

![RequestDefaults.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633533703437/aAZj70Gu8.png)

### Refactoring Postman syntax to JMX

We have to refactor the *ideaId* parameter so that it fits the Jmeter syntax. You can click on `Search` -> `Search` or the shortcut `Ctrl+F`. The search panel will appear. 

Now all we have to do is to change all the `:ideaId` to `${ideaId}`

Let's include an example ideaId value to our user variables. We could add it dynamically using JSON assertions, but it is out of the scope of the tutorial. If you want more info about it, [check this article.](https://www.blazemeter.com/blog/api-testing-with-jmeter-and-the-json-extractor)

The resultant User variables would be:

![UserVariables.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633533181364/0GmNO9xxa.png)

Finally, we have to add the defaultPath variable to each of the requests (I know, this is a heavy con 🥲🥲). If you find out any alternative, please let me know in the comments!


![DefaultPathImage.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633533157225/iJOSDCTtC.png)


### Add some assertions

We have our requests but, HOW THE HECK WE KNOW THEY ARE WORKING AS EXPECTED !?!?!?

> Use assertions

Let's add some assertions to the requests. 

- Right Click on any of the requests and select `Add` -> `Assertions` -> `Response Assertion`
- Select the Response Code and add the value `200`
- Change the `Pattern Matching Rules` to `Equals`


![Screenshot from 2021-10-06 17-18-39.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633533534345/d5IpYV1t-.png)

You can copy and paste the assertion to all the requests by doing `Ctrl+C` to the element, selecting the target request, and `Ctrl+V`.

Finally, we have to add a listener to the end of the suite to have a summary of the results of our testing process.

Click on the engine and select `Add` -> `Listener` and add both `View Results Tree` and `Assertion results`.

These two are the ones I use the most for testing myself. One will show you the data of the requests/response, very useful for debugging. The other one will show you the results of your assertions. The results tree would show anything different from a 2XX code as an error, but you may want to test error cases. That's when the `Assertion results` come in handy.

# Lauch your test suite

Now we are ready to test! 🥳🥳 

We didn't make a single line of code, remember. So, have to use the mock server we created in a previous article. For using it we have to set up the HTTP Request options to:

- Protocol: HTTPS
- Server: domain of your mock server. In my case ``
- Port: Empty or 80. It's the same.

Let's launch it!! Go and click on the RUN button in the toolbar. 

You should check your assertions and see EVERYTHING IS GREEN 💚 (Mock's magic 🪄🪄)

![Screenshot from 2021-10-06 17-11-13.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633533107934/TjbcVuOJU.png)

![Screenshot from 2021-10-06 17-11-23.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633533110876/BGCmcnXIB.png)


And that's it! You have created a test suite for your API design!

I know there are some alternatives like Newman which I have to test yet. But I find Jmeter to be very complete, as we have additional options, out of the scope of the article like:

- Checking DB rows
- Checking Redis/Cache status with custom Beanshell scripts
- JSON Assertions
- Way other things I don't know 😁😁


# Summary

In this article, I hope you have learned:

- Convert your Postman collection to a JMeter file using Loadium
- Install Jmeter 
- Configure a Jmeter file and add assertions
- Execute a Test Suite in Jmeter

![patrick-fun-gif](https://media.giphy.com/media/8WJw9kAG3wonu/giphy.gif?cid=ecf05e476rq397rl82lfotvyar9xwkkrl5wgzrk8p9mr1akx&rid=giphy.gif)

I am HONOURED if you have reached this point. Thanks a lot for reading 💙

Hopefully, I was able to explain how to create a Jmeter test file from a Postman, configure, and run it!

Reach out on [Twitter](https://twitter.com/VictorGarciaDev) to find more valuable content or just chatting!

🐦 [@victorgarciadev](https://twitter.com/VictorGarciaDev)

😼 [github.com/victorgrubio](https://github.com/victorgrubio)

### References

- [Complete Series](https://www.victorgarciar.com/series/api-design)
- [Blazemeter article](https://www.blazemeter.com/blog/how-get-started-jmeter-installation-test-plans)
- [Loadium site](https://loadium.com)
- [Jmeter docs](https://jmeter.apache.org/usermanual/index.html)

Thanks and Keep It Up! 🦾🦾