# AutessaScript

<html>
<style>
    .quote-container {
        background-color: #f4fafe;
        border-radius: 10px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        margin: 20px auto;
        max-width: 500px;
        padding: 20px;
    }
    .quote {
        font-style: italic;
        margin: 0;
        padding: 0;
    }
</style>
    <div class="quote-container">
        <div class="quote">
            <p>Because what we really needed was another coding language.</p>
        </div>
    </div>
</html>


## What is AutessaScript?
AutessaScript is a coding language designed by Autessa. It is an extension of JavaScript (this means that JavaScript works in the compiler) to provide utilities that are often used in enterprise. 

It is *easier* to use than JavaScript because there are built-in operations to turn multiple lines of code (ex. calling an API), into a single line with built-in error handling. 

> We are ALWAYS trying to improve AutessaScript - if there are utilities or functions you would like to see, please contact us and we'll make it happen!

AutessaScript integrates seamlessly with Autobot for rendering custom messages to a user. 

## Adding Logs
To add messages that print to the console (or output) in AutessaScript, use the Logger functionality. For example, if I wanted to write code that prints "Hello World", I would do the following:
```javascript
Logger.log("Hello World");
```

To see this in action, press the "Generate Output" button in Autobot in the test console of an AutessaScript experience block. 

These logs will **not** be shown in the end-user's experience.

## Rendering Plain Text
After you've done some computation and you want to display some text to the user, you will need to set the response. When you want to send a text response, you can use `response.setPlainTextResponse("hello!");`

For example:
```javascript
const randomNumber = Math.floor(Math.random() * 100);
response.setPlainTextResponse("Hello World. Here is a random number: " + randomNumber);
```

## Rendering HTML
Sometimes plain text is not enough to show the user what you want. For this situation, AutessaScript supports sending back an HTML response, which will then render in the bot. 

For example:
```javascript
const quoteContainerHtml = 
                "<html>" +
                "<style>" +
                "    .quote-container {" +
                "        background-color: #f4fafe;" +
                "        border-radius: 10px;" +
                "        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);" +
                "        padding: 20px;" +
                "    }" +
                "    .quote {" +
                "        font-style: italic;" +
                "        margin: 0;" +
                "        padding: 0;" +
                "    }" +
                "</style>" +
                "    <div class=\"quote-container\">" +
                "        <div class=\"quote\">" +
                "            <p>Hello world</p>" +
                "        </div>" +
                "    </div>" +
                "</html>";
response.setHtmlResponse(quoteContainerHtml);
```

## Common Script Examples

### Calling APIs
Calling APIs in AutessaScript is easy, with all error handling already taken care of for you!

#### GET
Here is how you can make a GET request in AutessaScript.
```javascript
var endpoint = "https://reqres.in/api/users/2";
var apiResponse = api.get(JSON.stringify(
{
    "endpoint": endpoint,
}
));
response.setPlainTextResponse(apiResponse);
```

If you wanted to include URI parameters or headers, you can do so easily with:
```javascript
var endpoint = "https://reqres.in/api/users";
var apiResponse = api.get(JSON.stringify(
{
    "endpoint": endpoint,
    // parameters accepts any URI elements that can be parsed like a string
    "parameters": {
        "page": 2
    },
    "headers": {
        "Content-Type": "application/json"
    }
}
));
response.setPlainTextResponse(apiResponse);
```

#### POST
Here is an example of how you can make a POST request in AutessaScript.
```javascript
const apiKey = "";
const payload = JSON.stringify({
    "modelId": "myModel",
    "utterance": "I want to buy some boots"
});
var apiResponse = api.post(JSON.stringify({
    "endpoint": "https://intent.autessa.com/predict",
    "payload": payload,
    "headers": {
        "Accept": "application/json",
        "Content-type": "application/json",
        "Authorization": `Bearer ${apiKey}`
    }
}));
Logger.log(apiResponse);
response.setPlainTextResponse(apiResponse);
```

### Embedding Forms
Let's suppose you wanted to embed a form in the bot for the user to play with. AutessaScript supports this through the HTML response.

Here is how you could create the [interactive form](autobot/inner/intent_model?id=defining-intents) to convert text to camel case.

```javascript
const html = 
                "<head>" +
                "    <style>" +
                "        .container {" +
                "            border: 4px solid #d2e4ec;" +
                "        }" +
                "    </style>" +
                "</head>" +
                "<body>" +
                "    <div class=\"container\">" +
                "        <form id=\"inputForm\">" +
                "            <input style=\"margin-top:10px\" type=\"text\" id=\"inputString\" required>" +
                "            <button class=\"button\" style=\"margin-top:10px;margin-bottom:10px\" onClick=\"updateOutput()\">Convert to Camel Case</button>" +
                "        </form>" +
                "        <p style=\"display:inline\" id=\"output\"></p>" +
                "        </p>" +
                "    </div>" +
                "    <script>" +
                "        function convertToCamelCase(inputString) {" +
                "            return inputString.replace(/(\\s|-|_)[a-z]/g, (match) => match.charAt(1).toUpperCase());" +
                "        }" +
                "" +
                "        function updateOutput(input) {" +
                "            const inputString = document.getElementById('inputString').value;" +
                "            const output = convertToCamelCase(inputString);" +
                "            document.getElementById('output').textContent = output;" +
                "        }" +
                "    </script>" +
                "</body>";
response.setHtmlResponse(html);
```

## FAQs

**I want to get inputs from the user in my AutessaScript. How can I do that?**

We are currently working on this feature to make it easier to accept user input in a bot conversation! However, we do support HTML (with script tags), iframes, and google forms in the meantime for getting information from users. Stay tuned for early 2024!

**There's functionality I want in the script that is not currently supported.**

Please contact us! We'd love to hear from you and add new features that our customers are looking for :)
