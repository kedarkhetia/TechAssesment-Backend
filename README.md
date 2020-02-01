# Expense Management System
***
In this project you will be creating a Expense Management System backend with RESTful APIs connected to a database. The project is divided in two parts. User APIs and Wallet APIs and you are required to create a schema for backend database where all the data will be stored. 

Aim of this project is to help you think about how to design backend systems and develop a prototype based on the design. You should plan to develop this project in the language and database which are more inline with your project choice. You are also not allowed to use any third party libraries without getting an approval from your instructor.

#### List of APIs: 
1. User APIs:
   * Registration
   Register user is a simple registration API. However, you should not allow duplicate users and must use [salting techniques](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/) to store the password in the database, you should also consider validating the email address.
    ```    
        HTTP Method: POST
        URL: /user/register
        Request Body: {
            "username": "JohnDoe",
            "password": "****",
            "email": "john@doe"
        }
        Success Code: 200 OK 
        Success Body: N/A.
        Error Code: 409 Conflict 
        Error Body: {
            "Message": "Username/Email already Exists."
        }
        Any other error: 400 Bad Request
    ```
   * Login
   User will use this api to login to the system. The API should return a session token as response. This session token will be used to query further APIs and identifying the user. From here on, for all the below APIs (i.e. from Logout), session-token must be passed in `Authorization` header in the request. To know more read about token based authentication.
    ```    
        HTTP Method: POST
        URL: /user/login
        Request Body: {
            "username": "JohnDoe",
            "password": "****"
        }
        Success Code: 200 OK 
        Success Body: {
            "session-token": "Au135hgGS!=="
        }
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "Invalid Username/Password"
        }
    ```
   * Logout
   User will use this api to logout of the system.
   ```    
        HTTP Method: POST
        URL: /user/logout
        Request Body: N/A.
        Success Code: 200 OK 
        Success Body: N/A.
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "No Active Session for provided session-token."
        }
    ```
   * Change Password
   User will call this api to change their id and password. As a extra step of verification, you should also verify that the old-password is actually the current password of the logged in user.
    ```    
        HTTP Method: PUT
        URL: /user/password
        Request Body: {
            "old-password": "****",
            "new-password": "****"
        }
        Success Code: 200 OK 
        Success Body: N/A.
        Error Code: 400 Bad Request
        Error Body-1: {
            "Message": "Invalid old-password."
        }
        Error Body-2: {
            "Message": "Old password and new password cannot be same."
        }
    ```
    * Show User Profile
    Displays user profile with average spending and remaining balance.
    ```    
        HTTP Method: GET
        URL: /user
        Request Body: N/A.
        Success Code: 200 OK 
        Success Body: {
            "username": "JohnDoe",
            "email": "john@doe",
            "remaining-balance": "$200",
            "average-spending": "$50/day",
            "expense-today": "$65"
        }
    ```
2. Wallet APIs:
    * Add Expense
    User will use this API to add an expense to his wallet. You should not allow user to add an expense if they do not have minimum balance. 
    ```    
        HTTP Method: POST
        URL: /wallet/expense/
        Request Body: {
            "title": "Pizza",
            "description": "Pizza from Dominos."
            "amount": "$12.15",
            "category": "FOOD",
            "date": "1/10/2020"
        }
        Success Code: 200 OK 
        Success Body: {
            "expense-id": "00112233-4455-6677-8899-aabbccddeeff"
        }
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "Cannot add expense. Remaining Balance is less than expense reported."
        }
    ```
    * Delete Expense
    User can delete the expense using this API. User should not be allowed to delete an expense if the expense-id doesn't exist in the database.
    ```    
        HTTP Method: DELETE
        URL: /wallet/expense/<expense-id>
        Request Body: N/A.
        Success Code: 200 OK 
        Success Body: N/A.
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "Expense Id provided does not exist."
        }
    ```
    * Edit Expense
    User can edit the expense using the is API.
    ```    
        HTTP Method: PUT
        URL: /wallet/expense/<expense-id>
        Request Body: {
            "title": "Pizza",
            "description": "Pizza from Dominos."
            "amount": "$12.15",
            "category": "FOOD",
            "date": "1/10/2020"
        }
        Success Code: 200 OK 
        Success Body: N/A.
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "Expense Id provided doesn't exist."
        }
    ```
    * View Expense
    User can view perticular expense using this API.
    ```    
        HTTP Method: GET
        URL: /wallet/expense/<expense-id>
        Request Body: N/A.
        Success Code: 200 OK 
        Success Body: {
            "title": "Pizza",
            "description": "Pizza from Dominos."
            "amount": "$12.15",
            "category": "FOOD",
            "date": "1/10/2020"
        }
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "Expense Id provided doesn't exist."
        }
    ```
    * Add Money
    User can add money to their wallet using this API.
    ```    
        HTTP Method: POST
        URL: /wallet/add
        Request Body: {
            "amount": "$12.15",
            "date": "1/10/2020"
        }
        Success Code: 200 OK 
        Success Body: N/A.
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "Expense Id provided doesn't exist."
        }
    ```
    * Get expenses between two dates.
    Display the list of expenses that were done between the two dates provided (including both start and end dates).
    ```
        HTTP Method: GET
        URL: /wallet/expense
        Request Body: {
            "start-date": "1/10/2020",
            "end-date": "1/15/2020"
        }
        Success Code: 200 OK 
        Success Body: [
            "00112233-4455-6677-8899-aabbccddeeff": {
                "title": "Pizza",
                "description": "Pizza from Dominos."
                "amount": "$12.15",
                "category": "FOOD",
                "date": "1/10/2020"
            },
            ...
            ...
        ]
        Error Code: 400 Bad Request
        Error Body: {
            "Message": "Invalid start date and end date."
        }
    ```
    * Get expense report by category and dates.
    ```
        HTTP Method: GET
        URL: /wallet/expense
        Request Body: {
            "start-date": "1/10/2020",
            "end-date": "1/15/2020",
            "category": "UTILITIES"
        }
        Success Code: 200 OK 
        Success Body: [
            "00112233-4455-6677-8899-aabbccddeeff": {
                "title": "Internet",
                "description": "Internet bill"
                "amount": "$75.23",
                "category": "UTILITIES",
                "date": "1/1/2020"
            },
            ...
            ...
        ]
        Error Code: 400 Bad Request
        Error Body-1: {
            "Message": "Invalid start date and end date."
        }
        Error Body-2: {
            "Message": "Invalid Category."
        }
    ```

