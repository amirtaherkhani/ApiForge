
# ApiForge

![ApiForge Logo](https://your-logo-url.com/logo.png)

---

## Table of Contents

- [Introduction](#introduction)
- [Key Features](#key-features)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage Example](#usage-example)
- [API Reference](#api-reference)
- [Environment Variables](#environment-variables)
- [Example Project Structure](#example-project-structure)
- [Why Choose ApiForge?](#why-choose-apiforge)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## Introduction

**ApiForge** is a powerful and flexible TypeScript package designed to serve as an efficient API gateway for your applications. It simplifies the process of integrating and managing external API calls within your application's routes, controllers, or services. By providing a streamlined interface for handling API interactions, ApiForge enhances developer productivity and ensures robust, maintainable code.

---

## Key Features

- **Seamless API Integration:** Effortlessly connect to multiple external APIs with support for various HTTP methods (GET, POST, PUT, DELETE).
- **Dynamic Parameter Handling:** Manage path variables, query parameters, and request bodies with ease.
- **Customizable Headers:** Define default headers and override them as needed for specific API requirements.
- **Robust Error Handling:** Comprehensive error management to gracefully handle and propagate API call failures.
- **Centralized Configuration:** Maintain a single configuration file for managing all your API endpoints, promoting scalability and maintainability.
- **Axios-Powered:** Leverages the reliability and performance of Axios for all HTTP requests, ensuring efficient communication with external services.
- **TypeScript Support:** Fully typed for enhanced developer experience and type safety.

---

## Installation

You can install ApiForge via npm or yarn:

```bash
# Using npm
npm install apiforge

# Using yarn
yarn add apiforge
```

```typescript
// <service-name>.config.ts
export default {
    baseUrl: 'https://api.example.com',
    endpoints: {
        UserService: [
            {
                name: 'getUserDetails',
                method: 'GET',
                path: '/users/{id}',
            },
            {
                name: 'createUser',
                method: 'POST',
                path: '/users',
            },
            // Add more endpoints as needed
        ],
        // Define other service groups as needed
    },
};
```

## Usage Example

Below is an example of how to use ApiForge within your TypeScript project to make API calls seamlessly.
Importing and Using apiRequest

```typescript
// Import the apiRequest function from ApiForge
import { apiRequest } from 'apiforge';

// Example: Fetching User Details
const getUserDetails = async (userId: string) => {
    try {
        const data = await apiRequest(
            'UserService',        // API group
            'getUserDetails',     // Endpoint name
            { id: userId },       // Path parameters
            { verbose: true },    // Query parameters (optional)
            undefined,            // Body parameters (optional)
            { Authorization: 'Bearer token' } // Custom headers (optional)
        );
        console.log('User Details:', data);
        return data;
    } catch (error) {
        console.error('Error fetching user details:', error);
        // Handle error appropriately
    }
};

// Example: Creating a New User
const createUser = async (userData: Record<string, any>) => {
    try {
        const data = await apiRequest(
            'UserService',
            'createUser',
            undefined,
            undefined,
            userData, // Body parameters
            { Authorization: 'Bearer token' }
        );
        console.log('User Created:', data);
        return data;
    } catch (error) {
        console.error('Error creating user:', error);
        // Handle error appropriately
    }
};

```
```typescript

// Usage in a Controller or Service

// src/controllers/userController.ts
import { Request, Response } from 'express';
import { getUserDetails, createUser } from '../services/userService';

export const userController = {
    async show(req: Request, res: Response) {
        const userId = req.params.id;
        try {
            const userDetails = await getUserDetails(userId);
            res.json(userDetails);
        } catch (error) {
            res.status(error.status || 500).json({ message: error.details });
        }
    },
    
    async store(req: Request, res: Response) {
        const userData = req.body;
        try {
            const newUser = await createUser(userData);
            res.status(201).json(newUser);
        } catch (error) {
            res.status(error.status || 500).json({ message: error.details });
        }
    },
};
```

## API Reference

  A helper function to send HTTP requests to configured API endpoints.
	Signature:
```typescript
apiRequest(
    group: string,
    endpointName: string,
    pathParams?: Record<string, string>,
    queryParams?: Record<string, any>,
    bodyParams?: Record<string, any>,
    headers?: Record<string, string>
): Promise<any>
```
**Parameters:**

   - group (string): The API group name, e.g., 'UserService', 'OrderService'.
   - endpointName (string): The specific endpoint name defined in the configuration, e.g., 'getUserDetails', 'createUser'.
   - pathParams (optional, Record<string, string>): Parameters to replace path variables in the endpoint URL, e.g., { id: '123' }.
   - queryParams (optional, Record<string, any>): Query parameters to append to the URL, e.g., { verbose: true }.
   - bodyParams (optional, Record<string, any>): The request body for methods like POST, PUT, DELETE.
   - headers (optional, Record<string, string>): Custom headers to include in the request.

**Returns:**

	Promise<any>: Resolves with the response data from the API call or rejects with an error object containing status and details.
	
**Error Handling:**

Errors are thrown with an object containing:

   - status (number): HTTP status code or 500 for unknown errors.
   - details (any): Detailed error information from the API response or a generic message.

**Why Choose ApiForge?**

>    **Simplicity**: Reduces boilerplate code for API integrations,
> enabling developers to focus on core functionalities.
>     **Maintainability**: Centralized configuration makes it easy to manage and update API endpoints.
>     **Scalability**: Supports complex API interactions, suitable for both small projects and large-scale applications.
>     **Reliability**: Built on top of Axios, ensuring robust and performant HTTP requests.
>     Type Safety: Fully typed with TypeScript, enhancing developer experience and reducing runtime errors.


Please ensure that your contributions adhere to the project's coding standards and include appropriate tests.
License

***ApiForge is released under the MIT License.
Contact***

For any questions, suggestions, or support, please open an issue on the GitHub repository or contact the maintainer at amirtaherkhani@outlook.com.


## **Example Usage in a Full Application**

Here's a more comprehensive example demonstrating how ApiForge can be integrated into a typical Express.js application.
Configuration
```typescript
    // src/your-service.config.ts
    export default {
        baseUrl: 'https://api.example.com',
        endpoints: {
            UserService: [
                {
                    name: 'getUserDetails',
                    method: 'GET',
                    path: '/users/{id}',
                },
                {
                    name: 'createUser',
                    method: 'POST',
                    path: '/users',
                },
            ],
            // Add other service groups as needed
        },
    };
```
**Service Layer**
```typescript
    // src/services/userService.ts
    import { apiRequest } from 'apiforge';
    
    export const getUserDetails = async (userId: string) => {
        return await apiRequest(
            'UserService',
            'getUserDetails',
            { id: userId }
        );
    };

    export const createUser = async (userData: Record<string, any>) => {
        return await apiRequest(
            'UserService',
            'createUser',
            undefined,
            undefined,
            userData
        );
    };
```
