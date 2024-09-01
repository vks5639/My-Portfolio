# API Testing with Postman

## About the Project

This project focuses on automating the testing of an API for an e-commerce platform's cart module. The API allows operations such as creating a session, adding products to the cart, viewing cart contents, editing product quantities, and removing products from the cart. The tests were organized into a collection in Postman, which was then executed through various methods, including Newman (Postman's command-line tool), Postman’s shared URL, GitHub integration, and Jenkins automation.

## What Was Done

### 1. **Creating Postman Collection**
   - A Postman collection was created to group the API requests related to the cart module of the e-commerce platform.
   - The collection included the following API requests:
     - **Create Session/Token**: Initializes a session by generating a token.
     - **Add Product to Cart**: Adds a specified product to the cart.
     - **View Cart Content**: Retrieves the current contents of the cart.
     - **Edit Product Quantity in Cart**: Modifies the quantity of a product already in the cart.
     - **Remove Product from Cart**: Removes a specified product from the cart.
       
![](https://i.imgur.com/kv06Bei.png)   
  
![](https://i.imgur.com/8nbkiGl.png)

![](https://i.imgur.com/UaamYLg.png)

### 2. **Automating Tests Using Newman**
   - Newman was used to run the Postman collection from the command line.
   - **Report Summary**:
     - **Total Iterations**: 10
     - **Total Requests**: 50
     - **Total Assertions**: 60
     - **Average Response Time**: 115ms
     - **Total Failures**: 0

![](https://i.imgur.com/D2mUeHO.png)

### 3. **Running Tests via Postman Shared URL**
   - The collection was shared through a Postman URL, allowing others to execute the tests directly from their browser without requiring the Postman desktop application.

![](https://i.imgur.com/m82UOSf.png)

### 4. **Integration with GitHub**
   - The Postman collection and Newman scripts were committed to a GitHub repository.
   - This setup allows for continuous integration, where the API tests can be automatically triggered on code changes.

![](https://i.imgur.com/5jcvmJ0.png)

### 5. **Automating with Jenkins**
   - Jenkins was used to automate the execution of the API tests on a scheduled basis or in response to specific events (e.g., code commits).
   - Jenkins was configured to pull the latest collection from GitHub and execute it using Newman.

![](https://i.imgur.com/Pic8qSW.png)     

## Conclusion

This project demonstrates the power and flexibility of Postman and Newman for API testing and automation. By creating a structured Postman collection, automating tests through Newman, and integrating with GitHub and Jenkins, we ensured that the e-commerce platform's cart module API is robust and reliable. The zero failures reported in the test results highlight the stability of the API under the tested conditions.

The use of various tools and platforms in this project emphasizes the importance of continuous testing and integration in maintaining high-quality APIs in a production environment.

---


