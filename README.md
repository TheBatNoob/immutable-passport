# Integrating Immutable Passport into Your Application

Immutable Passport is a powerful tool for integrating user authentication, wallet management, and digital asset transactions into your web3 application. In this guide, we'll take you through the steps to integrate Immutable Passport into your application. Here's what we'll cover:

1. Creating a Simple Application: We'll start by creating a simple web application. If you already have an application, you can skip this step.

2. Registering the Application: You need to register your application on the Immutable Developer Hub to obtain the required credentials.

3. Installing and Initializing the Passport Client: We'll guide you through the process of installing the Immutable Passport client and initializing it.

4. Logging In a User with Passport: Learn how to implement user login functionality using Immutable Passport.

5. Displaying User Information: We'll show you how to display the user's ID token, access token, and nickname after authentication.

6. Logging Out a User: Implement a user logout feature for enhanced security.

7. Initiating a Transaction: Learn how to initiate a transaction, such as sending a placeholder string and obtaining the transaction hash.

Prerequisites
Before you begin, ensure you have the following prerequisites:

Basic knowledge of web development and JavaScript.
Node.js installed on your machine.
A code editor (e.g., Visual Studio Code).

# 1. Creating a Simple Application
If you don't have an existing application, let's create a basic one. You can also skip this step if you already have an application.

    mkdir passport-integration
    cd passport-integration

Initialize a new Node.js application
    npm init -y

# 2. Registering the Application
Before using Passport, you must register your application as an OAuth 2.0 client in the Immutable Developer Hub. First, you'll need to create a project and a testnet environment. Then you can navigate to the Passport config screen and create a passport client for your created environment.
---Property	---Description

|Application Type	The type of your application. At the moment, only the Web Application option is available. A Native option for mobile or desktop applications is coming soon.
|Client name	The name you wish to use to identify your application.
|Logout URLs	The URL that your users will be redirected to upon logging out of your application.
|Callback URLs	The URL that your users will be redirected to once the authentication is complete. This is also where your application processes the authentication response.
|Web Origins URLs	The URLs that are allowed to request authorization. This field is available when you select the Native application type.
