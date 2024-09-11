# Postman Training - Day 1

This training focuses on using Postman for API testing. We'll use the [Simple Grocery Store API](https://github.com/vdespa/Postman-Complete-Guide-API-Testing/blob/main/simple-grocery-store-api.md) as an example throughout the training.

## 1. How to Install Postman
- Visit the [Postman website](https://www.postman.com/downloads/) to download the desktop application.
- Postman is available for macOS, Windows, and Linux.
- After installation, create a free account or log in if you already have one.
- Optionally, Postman is also available as a web application.

## 2. What is an API?
- API stands for Application Programming Interface.
- APIs allow two applications to communicate with each other.
- They expose data and functionality via endpoints that can be accessed programmatically over the web.
- Example: The Simple Grocery Store API allows you to manage grocery store carts and orders through HTTP requests.

## 3. First API Request Using Postman (GET)
- Open Postman and create a new request.
- Choose `GET` as the request method.
- Enter the following URL: `https://simple-grocery-store-api.glitch.me/status`.
- Click **Send** to make the request.
- You should receive a response indicating the API status: `{ "status": "UP" }`.

## 4. Troubleshooting Postman Errors Using the Console
- Open the Postman Console (`View` -> `Show Postman Console`).
- Use the console to view detailed logs for each request.
- Logs show headers, request body, status codes, and errors.
- The console is helpful for debugging when requests fail or behave unexpectedly.

## 5. Using Postman on Web with Desktop Agent, Cloud Agent, or Browser Agent
- Postman can be used in the browser with cloud agents or the desktop agent.
- Cloud agent: Handles requests directly from the Postman web interface, with no local setup needed.
- Desktop agent: Used to send requests from the web version while utilizing the local network (recommended for private APIs).
- Choose the appropriate agent depending on the network setup and privacy needs.

## 6. Postman Collections
- Collections are groups of saved API requests.
- They help organize related requests and automate workflows.
- Create collections to manage sets of API requests, like all product and cart-related requests in the grocery store API.

## 7. Storing Configuration in Collection Variables (baseUrl) - Initial Value and Current Value
- Collection variables allow storing dynamic values like `baseUrl`.
- `baseUrl` can store `https://simple-grocery-store-api.glitch.me`, which can be referenced in all requests.
- The initial value is stored in the collection and visible to team members, while the current value is local to your environment.
- Example: Use `{{baseUrl}}/status` instead of hardcoding the full URL.

## 8. GET Request
- A GET request retrieves data from the server.
- Example: To get all products from the grocery store, use the endpoint: `GET /products`.
- In Postman, enter the URL `{{baseUrl}}/products` and click **Send**.

## 9. Visualizing Responses in Postman
- **Body**: Displays the data returned by the API in different formats (JSON, XML, etc.).
- **Headers**: Show metadata about the response, including content type and status code.
- **Console**: Provides logs with detailed request and response information, useful for debugging.

## 10. Query Parameters
- Query parameters are added to the URL to filter data.
- Example: `GET /products?category=coffee` filters products by category.
- You can add query parameters in Postman by clicking **Params** and adding key-value pairs.

## 11. Path Parameters
- Path parameters are part of the URL used to identify specific resources.
- Example: `GET /products/:productId` retrieves a product by its ID, like `GET /products/4643`.

## 12. Query Parameters vs Path Parameters
- **Query Parameters**: Optional parameters used to filter or modify the request, typically added after `?` in the URL.
- **Path Parameters**: Required as part of the URL to identify specific resources, like an ID in `/products/:productId`.

## 13. JSON Format Example
- JSON (JavaScript Object Notation) is a lightweight data format.
- Example of a JSON response:
    ```json
    {
        "id": 4643,
        "name": "Starbucks Coffee",
        "category": "coffee",
        "inStock": true
    }
    ```

## 14. POST Request with JSON (Create Cart)
- POST requests send data to the server to create new resources.
- Example: To create a new cart, send an empty POST request to `{{baseUrl}}/carts`.
- The server responds with a new `cartId`.

## 15. API Authentication
- Some APIs require authentication via tokens.
- Example: Register a new API client to get an access token by sending a POST request to `{{baseUrl}}/api-clients`.
- Use the token in the Authorization header: `Authorization: Bearer <token>`.

## 16. POST Request with API Authentication (Create Order)
- After authentication, use your token to create a new order.
- Example: Send a POST request to `{{baseUrl}}/orders` with a JSON body containing `cartId` and `customerName`.

## 17. PATCH Request - Update Cart or Order
- PATCH requests modify existing resources.
- Example: To update the quantity of an item in a cart, send a PATCH request to `{{baseUrl}}/carts/:cartId/items/:itemId` with the new quantity in the body.

## 18. PUT Request
- PUT requests replace an entire resource with new data.
- Example: Replace an item in the cart using `PUT /carts/:cartId/items/:itemId` with `productId` and `quantity`.

## 19. DELETE Request
- DELETE requests remove resources from the server.
- Example: To delete an item from the cart, send a DELETE request to `{{baseUrl}}/carts/:cartId/items/:itemId`.

---

# Postman Training - Day 2

This training will focus on advanced testing features in Postman, including scripting, assertions, and handling different scenarios.

## 1. Simple Postman Script to Validate Response is 200 OK

You can use Postman’s scripting feature to validate that the response from an API request is 200 OK. Add the following script in the **Tests** tab of your request.

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- This script checks if the status code returned by the API is 200 and will pass the test if the condition is true.

## 2. Testing the Response Body with Postman Assertions

You can validate the structure and content of the response body using Postman assertions. Here's an example of checking whether a specific field in the JSON response exists.

In the **Tests** tab, use this script:

```javascript
pm.test("Check if 'status' field exists in response", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('status');
});
```

- This script checks if the `status` field is present in the response.

For more detailed documentation on Postman assertions, visit the [Postman Assertions Documentation](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/).

## 3. Testing API Error Handling

To test how the API handles errors, you can simulate invalid requests and verify the error response. For example, send a request to an invalid endpoint or with incorrect data and use assertions to verify the error code.

Example script to validate error response:

```javascript
pm.test("Status code is 404", function () {
    pm.response.to.have.status(404);
});

pm.test("Response should contain error message", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
});
```

- This script checks if the status code is 404 and if the response contains an `error` field.

## 4. Testing Positive and Negative Scenarios

It’s important to test both positive (valid) and negative (invalid) scenarios to ensure the API behaves correctly in all cases.

**Positive Scenario:**

For a valid scenario, expect the correct data to be returned. For example:

```javascript
pm.test("Status code is 200 for valid request", function () {
    pm.response.to.have.status(200);
});

pm.test("Response contains expected data", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('id');
    pm.expect(jsonData.id).to.eql(4643);
});
```

**Negative Scenario:**

For invalid requests, ensure the API responds with the appropriate error:

```javascript
pm.test("Status code is 400 for invalid request", function () {
    pm.response.to.have.status(400);
});

pm.test("Response contains error message", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
});
```

- Positive testing checks the API’s response with valid data, and negative testing checks its response with invalid data.

## 5. Using Postman Variables

Variables in Postman allow you to store and reuse values in your requests. You can use variables to store URLs, request bodies, headers, and more.

To set a variable in Postman, use the following in the **Pre-request Script** or **Tests** tab:

```javascript
pm.environment.set("myVariable", "someValue");
```

- This script sets an environment variable called `myVariable` with the value `someValue`.

To use the variable in a request, refer to it using double curly braces `{{myVariable}}`:

```text
{{myVariable}}
```

## 6. Setting Collection Variables

Collection variables allow you to define variables that are accessible to all requests within a collection.

To set a collection variable, use the following script in the **Tests** tab:

```javascript
pm.collectionVariables.set("baseUrl", "https://simple-grocery-store-api.glitch.me");
```

- This sets a collection variable `baseUrl` with the value of the API’s base URL.

## 7. Getting Collection Variables

To retrieve and use the value of a collection variable in your script or request, use the following syntax:

```javascript
let baseUrl = pm.collectionVariables.get("baseUrl");
console.log(baseUrl); // Logs the value of baseUrl
```

- This retrieves the value of `baseUrl` from the collection variables and logs it in the Postman console.


Here is the complete **README.md** for Day 3, with all topics and examples fully inside the file as requested:

# Postman Training - Day 3

This training will focus on advanced Postman features, including environments, variables, collection runners, and Newman.

## 1. Postman Environments and Environment Variables

- Environments in Postman allow you to define variables that can be used across multiple requests.
- Environment variables are useful for storing values such as API keys, URLs, or any other data that might change between requests or environments.
- You can create different environments like `Development`, `Staging`, and `Production`, each containing different values for the same variables (e.g., `baseUrl`).

Example of using an environment variable in the URL:

```text
{{baseUrl}}/products

- You can create and manage environments in the **Environments** section of Postman.

## 2. Storing Secrets in Environment

- Environment variables can also be used to store sensitive information such as API keys and tokens.
- Postman provides a way to set **initial values** (shared across teams) and **current values** (local to your session). Sensitive information should be set as **current values** to keep them private.

For example, to store an API key:

1. Go to **Environments**.
2. Add a new environment variable `apiKey` and set the value.
3. Use `{{apiKey}}` in the request headers or body.

Example of setting an API key in the headers:

```text
Authorization: Bearer {{apiKey}}
```

## 3. Exporting and Importing Environments

- You can export an environment by selecting the **Manage Environments** option in Postman and clicking **Download** next to the environment you wish to export.
- To import an environment, click **Import**, select the exported `.json` file, and Postman will add it to your environments.

## 4. Setting, Getting, and Removing Environment Variables from Scripts

You can manipulate environment variables directly from your scripts. For example, you can set, get, and remove environment variables in the **Tests** or **Pre-request Script** tabs.

### Setting an Environment Variable

```javascript
pm.environment.set("token", "mySecretToken");
```

### Getting an Environment Variable

```javascript
let token = pm.environment.get("token");
console.log(token);
```

### Removing an Environment Variable

```javascript
pm.environment.unset("token");
```

## 5. Variables Scope

Postman supports variables at different scopes:

- **Global**: Accessible throughout Postman.
- **Environment**: Available to specific environments.
- **Collection**: Variables specific to a collection.
- **Local**: Variables specific to a request or a script.
- **Data**: Variables used when running collections with the collection runner.

## 6. Global Variables

- Global variables are accessible across all environments and requests in Postman.
- To set a global variable, use the following script:

```javascript
pm.globals.set("globalVar", "globalValue");
```

- To retrieve a global variable:

```javascript
let globalVar = pm.globals.get("globalVar");
console.log(globalVar);
```

## 7. Running Postman Collection with Collection Runner

- The Postman **Collection Runner** allows you to run all the requests in a collection.
- To run a collection, go to **Collection Runner**, choose the collection, and click **Run**.
- You can run collections against different environments, and with different sets of data.

## 8. Collection Runner Usage Limits

- Postman’s free tier has limitations on how many requests you can run with the Collection Runner.
- If you exceed these limits, you may need to upgrade to a paid plan for more runs or consider using Newman.

## 9. Scheduled Collection Runs

- Postman allows you to schedule collection runs using **Postman Monitors**.
- You can set up monitors to run collections periodically (e.g., every 5 minutes, hourly, daily).
- Monitors can be managed from the **Monitors** tab, and you can track their performance and results.

## 10. What is Newman?

- **Newman** is a command-line tool that allows you to run Postman collections outside of Postman (e.g., in a CI/CD pipeline).
- It provides a way to automate collection runs and integrates easily into build systems like Jenkins, Travis CI, or GitHub Actions.

## 11. Newman Installation

To install Newman, use the following command (Node.js is required):

```bash
npm install -g newman
```

## 12. Running Collection with Newman Using Collection URL

You can run a collection directly from a Postman shareable URL using Newman:

```bash
newman run https://www.getpostman.com/collections/{{your-collection-id}}
```

- Replace `{{your-collection-id}}` with your actual Postman collection URL.

## 13. Running Collection with Local File in Newman

You can also run a local Postman collection file (in `.json` format) with Newman:

```bash
newman run /path/to/collection.json
```

- Ensure the file is exported from Postman before running the command.

## 14. Specifying Environments in Newman

You can specify an environment file when running a collection with Newman:

```bash
newman run /path/to/collection.json -e /path/to/environment.json
```

- This will apply the environment settings from the `.json` file to the collection run.

## 15. Installing and Generating htmlextra Reports

Newman can generate detailed HTML reports using the `htmlextra` reporter.

1. First, install the `htmlextra` reporter:

```bash
npm install -g newman-reporter-htmlextra
```

2. Then, run a collection with the reporter:

```bash
newman run /path/to/collection.json -r htmlextra
```

- This will generate an HTML report with detailed information about the collection run.

## 16. Postman Workflows - How to Change Execution Flow with `pm.execution.setNextRequest` and Skip Request

You can control the flow of requests in Postman by using the `pm.execution.setNextRequest()` function.

### Changing Execution Flow

To change the order of execution, use the following script in the **Tests** tab of a request:

```javascript
pm.execution.setNextRequest("Next Request Name");
```

- This will make Postman skip any intermediate requests and directly execute the request with the name `"Next Request Name"`.

### Skipping a Request

To skip the current request and move to the next one in the collection, use:

```javascript
pm.execution.skip();
```

- This will skip the current request, and Postman will continue with the next one in sequence.
```





