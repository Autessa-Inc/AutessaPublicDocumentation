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

AutessaScript integrates seamlessly with Autessa and Autobot to create any flow you can think of. 

## Rendering Final Output
All AutessaScript in a tool executor requires a string response to be set, and that will be treated as the return value of the script. To do this, add the following line:
```javascript
response.setResponse("Hello World");
```

## Adding Logs
To add messages that print to the console (or output) in AutessaScript, use the Logger functionality. For example, if I wanted to write code that prints "Hello World", I would do the following:
```javascript
Logger.log("Hello World");
```

These logs will **not** be shown in the end-user's experience.


## Accessing Variables
To access variables in AutessaScript, you can use the `variables` object. For example, if you wanted to access a variable called `myVariable`, you would do the following:
```javascript
const myVariable = variables.getVariable("myVariable");
```

Sometimes, depending on how you want to use the variable (ex. equality), you may need to case it. So, for example:
```javascript
const myVariable = variables.getVariable("myVariable");
const varAsString = String(myVariable);
if (varAsString === "hello") {
    Logger.log("The variable is hello!");
}
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
response.setResponse(apiResponse);
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
response.setResponse(apiResponse);
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
response.setResponse(apiResponse);
```

### Sending An Email
Behind the scenes, we use MailerSend as our client to send emails. We do not support batch emails being sent - emails should be more for notifications rather than newsletters.

#### Sending automated email 
This will send an email from emails@autessa.com to the recipient specified in the JSON object.
```javascript
apiResponse = mailer.sendEmail(JSON.stringify({
    "to": "test@gmail.com",
    "subject": "Test Email from AutessaScript",
    "text": "This is a test email from AutessaScript",
}));

Logger.log(apiResponse)
```

#### Creating an HTML email
Sometimes, you may have more complicated email types that you want to show when the email renders
```javascript
apiResponse = mailer.sendEmail(JSON.stringify({
    "to": "test@gmail.com",
    "subject": "Test Email from AutessaScript",
    "html": "<h2>This is a test email from AutessaScript</h2>",
}));

Logger.log(apiResponse)
```


### Calling Built-In LLM
While you can use API calls to call external LLMs, we also support accessing our built-in LLM with ease.
```javascript
response = genai.getResponse(JSON.stringify({
    "prompt": "Tell me a joke",
    "system_prompt": "You're a funny stand-up comedian."
}));

Logger.log(response);
```

#### Passing History
Sometimes, you may want to include additional conversational context to an LLM. You can do this by passing the history object. For example:
```javascript
response = genai.getResponse(JSON.stringify({
    "prompt": "Tell me a joke",
    "system_prompt": "You're a funny stand-up comedian.",
    "history": [
        {
            "role": "user",
            "content": "I really love giraffes"
        },
        {
            "role": "assistant",
            "content": "Excellent, here are some facts about giraffes!"
        }
    ]
}));
```


## FAQs

**There's functionality I want in the script that is not currently supported.**

Please contact us! We'd love to hear from you and add new features that our customers are looking for :)
