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

```markdown
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
```




