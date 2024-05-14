# Classic vs Modern REST API Design Options
The ServiceBricks platform allows you to choose how to fundamentally expose your REST API methods from your services by using a simple configuration option using either Classic or Modern mode.

**Note: You must set both the server and client configurations to match.**

## Classic Mode
The standard, vanilla REST API services pattern we were brought up on had only a few standards. 
When you call a GET method/verb to a route like Person/1, you receive a serialized object for a person with id = 1 as the response.
```json
{
  "id": 1,
  "name": "John"
}
```
If an error occured, you received a 500 internal server error. Later, rather than throwing exceptions, the pattern changed to use a ProblemDetails object to return better messages to clients.
This is what I refer to as **Classic** mode.

## Modern mode
Several years ago I began designing API methods that returned response objects, rather than using standard serialized objects.  This is what I refer to as **Modern** mode.

A response object consist of the following things:
* A success or error disposition - This simply lets you know if the call was successful or not.
* A list of response messages - This list of messages pertaining to the call. These messages have a message and a severity associated to them, such as error, warning, info, etc. This way you can let the user know something.
* A response item (option) - A single object to return
* A response list (option) - A list of objects to return

```json
{
  "Success": true,
  "Messages": [
    {
      "type": "warning",
      "message": "The system will be down for maintenance in 22 minutes."
    }
  ]
  "Item":{
    "id": 1,
    "name": "John"
  }
}
```

One could just return a list of objects every time instead of a response item, for further refactoring down. I've personally disliked having to access an array instead of a single item property. 
Call it personal preference or just laziness for less code to write on the receiving end.

## Server Configuration
You configure the mode you want by using the appsettings.json file with your application. 
Setting the ServiceBricks:Api:ReturnResponseObject property allows you to change how all services respond on the web server.
```json
{
  "ServiceBricks":  {
    "Api": {      
      // Classic = false, Modern = true
      "ReturnResponseObject": true
    },
  }
}
```

## Client Configuration
You configure the mode you want for each microservice by using the appsettings.json file with your client application.
Setting the ServiceBricks:Client:ApiConfig:ReturnResponseObject property allows you to change how the web server will respond.

```json
{
  "ServiceBricks": {
    "Client": {
      "ApiConfig": {
        // Classic = false, Modern = true
        "ReturnResponseObject": true,
      }
    }
  }
}
```
