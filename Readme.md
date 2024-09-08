# Postman Training - Day 1

This training will focus on using Postman for API testing. We'll use the [Simple Grocery Store API](https://github.com/vdespa/Postman-Complete-Guide-API-Testing/blob/main/simple-grocery-store-api.md) as an example throughout the training.

## 1. How to Install Postman
- Visit [Postman website](https://www.postman.com/downloads/) to download the desktop application.
- Postman is available for macOS, Windows, and Linux.
- After installation, create a free account or log in if you already have one.
- Optionally, Postman is also available as a web application.

## 2. What is an API
- API stands for Application Programming Interface.
- APIs allow two applications to communicate with each other.
- They expose data and functionality via endpoints that can be accessed programmatically over the web.
- Example: Simple Grocery Store API allows you to manage grocery store carts and orders through HTTP requests.

## 3. First API request using Postman (GET)
- Open Postman and create a new request.
- Choose `GET` as the request method.
- Enter the following URL: `https://simple-grocery-store-api.glitch.me/status`.
- Click **Send** to make the request.
- You should receive a response indicating the API status: `{ "status": "UP" }`.

## 4. Troubleshooting Postman errors using console
- Open Postman Console (`View` -> `Show Postman Console`).
- Use the console to view detailed logs for each request.
- Logs show headers, request body, status codes, and errors.
- Helpful for debugging when requests fail or behave unexpectedly.

## 5. Using Postman on web with desktop agent, cloud agent, or browser agent
- Postman can be used in the browser with cloud agents or the desktop agent.
- Cloud agent: Handles requests directly from the Postman web interface, no local setup needed.
- Desktop agent: Used to send requests from the web version while using the local network (recommended for private APIs).
- Choose the appropriate agent depending on the network setup and privacy needs.

## 6. Postman Collections
- Collections are groups of saved API requests.
- They help organize related requests and automate workflows.
- Create collections to manage sets of API requests, like all product and cart-related requests in the grocery store API.

## 7. Storing configuration in collection variables (baseUrl) - initial value and current value
- Collection variables allow storing dynamic values like `baseUrl`.
- `baseUrl` can store `https://simple-grocery-store-api.glitch.me`, which can be referenced in all requests.
- Initial value is stored in the collection and visible to team members, while current value is local to your environment.
- Example: `{{baseUrl}}/status` can be used instead of hardcoding the full URL.

## 8. GET Request
- A GET request retrieves data from the server.
- Example: To get all products from the grocery store, use the endpoint: `GET /products`.
- In Postman, enter the URL `{{baseUrl}}/products` and click **Send**.

## 9. Visualizing responses in Postman - in body, headers, and in console
- Body: Displays the data returned by the API in different formats (JSON, XML, etc.).
- Headers: Show metadata about the response, including content type and status code.
- Console: Provides logs with detailed request and response information, useful for debugging.

## 10. Query Parameters
- Query parameters are added to the URL to filter data.
- Example: `GET /products?category=coffee` filters products by category.
- You can add query parameters in Postman by clicking **Params** and adding key-value pairs.

## 11. Path Parameters
- Path parameters are part of the URL to identify specific resources.
- Example: `GET /products/:productId` retrieves a product by its ID, like `GET /products/4643`.

## 12. Query Parameters vs Path Parameters
- **Query Parameters**: Optional parameters to filter or modify the request, usually added after the `?` in the URL.
- **Path Parameters**: Required as part of the URL to identify specific resources, like an ID in `/products/:productId`.

## 13. JSON Format with a small example
- JSON (JavaScript Object Notation) is a lightweight data format.
- Example of JSON response:
    ```json
    {
            "id": 4643,
            "name": "Starbucks Coffee",
            "category": "coffee",
            "inStock": true
    }
    ```

## 14. POST Request with JSON (create cart)
- POST requests send data to the server to create new resources.
- Example: To create a new cart, send an empty POST request to `{{baseUrl}}/carts`.
- The server responds with a new cartId.

## 15. API Authentication
- Some APIs require authentication via tokens.
- Example: Register a new API client to get an access token by sending a POST request to `{{baseUrl}}/api-clients`.
- Use the token in the Authorization header: `Authorization: Bearer <token>`.

## 16. POST Request with API authentication (create order)
- After authentication, use your token to create a new order.
- Example: Send a POST request to `{{baseUrl}}/orders` with a JSON body containing cartId and customerName.

## 17. PATCH Request - Update cart and order
- PATCH requests modify existing resources.
- Example: To update the quantity of an item in a cart, send a PATCH request to `{{baseUrl}}/carts/:cartId/items/:itemId` with the new quantity in the body.

## 18. PUT Request
- PUT requests replace an entire resource with new data.
- Example: Replace an item in the cart using `PUT /carts/:cartId/items/:itemId` with productId and quantity.

## 19. DELETE Request
- DELETE requests remove resources from the server.
- Example: To delete an item from the cart, send a DELETE request to `{{baseUrl}}/carts/:cartId/items/:itemId`.