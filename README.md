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
Before using Passport, you must register your application as an OAuth 2.0 client in the [Immutable Developer Hub](https://hub.immutable.com/). First, you'll need to create a project and a testnet environment. Then you can navigate to the Passport config screen and create a passport client for your created environment.
# There are a few crucial details that must be provided when adding a client:
a. Application Type - The type of your application. At the moment, only the Web Application option is available. A Native option for mobile or desktop applications is coming soon.
b. Client name - The name you wish to use to identify your application.
c. Logout URLs - The URL that your users will be redirected to upon logging out of your application.
d. Callback URLs - The URL that your users will be redirected to once the authentication is complete. This is also where your application processes the authentication response.
e. Web Origins URLs - The URLs that are allowed to request authorization. This field is available when you select the Native application type.

Once you have successfully registered your application in the Immutable Developer Hub. Be sure to make a note of your application's Client ID, Callback URL and the Logout URL that you entered, as you will need these to initialise the Passport module in your application.

# 3. Installing and initialising the Passport client
Install the Immutable SDK:
Now that you have a sample web application project, you can install the Immutable SDK as a dependency. To do this, run the following command in your project's root directory:

    npm install -D @imtbl/sdk
In your sample web application, create a JavaScript file (e.g., app.js) where you will initialize the Passport client and implement Passport functionalities. Here's how to initialize Passport in your app.js:

    import { config, passport } from '@imtbl/sdk';
    
    const passportInstance = new passport.Passport({
      baseConfig: new config.ImmutableConfiguration({
        environment: config.Environment.PRODUCTION,
      }),
      clientId: '<YOUR_CLIENT_ID>',
      redirectUri: 'https://example.com',
      logoutRedirectUri: 'https://example.com/logout',
      audience: 'platform_api',
      scope: 'openid offline_access email transact',
    });
Make sure to replace <YOUR_CLIENT_ID> with the actual client ID you obtained when registering your application in the Immutable Developer Hub.
With the Passport client initialized in your sample application, you can now proceed to implement Passport functionalities in your app.

# 4. Logging in a user with Passport
Create a new JavaScript file, e.g., passport-login.js, and add the login process functions to it. Here's the code for passport-login.js:

    import { ethers } from 'ethers';
    
    // Initialize Passport provider
    const initializePassportProvider = () => {
      return passportInstance.connectEvm();
    };
    
    // Trigger the login process
    const triggerLogin = async (passportProvider) => {
      try {
        const accounts = await passportProvider.requestAccounts();
        return accounts[0];
      } catch (error) {
        console.error('Login failed:', error);
        return null;
      }
    };
    
    // Configure the login callback
    const configureLoginCallback = () => {
      window.addEventListener('load', function() {
        passport.loginCallback();
      });
    };
    
    export { initializePassportProvider, triggerLogin, configureLoginCallback };
In your sample app's main script(app.js) , import the functions from passport-login.js:
    
    import { initializePassportProvider, triggerLogin, configureLoginCallback } from './passport-login.js';
    
    // Other app logic
    
    // Initialize Passport provider and configure login callback
    const passportProvider = initializePassportProvider();
    configureLoginCallback();
    
    // When the user clicks a "Login" button or similar action
    const userAddress = await triggerLogin(passportProvider);
    
    if (userAddress) {
      console.log('User logged in with address:', userAddress);
      // Proceed with application logic here.
    } else {
      console.log('Login process failed');
    }

Customize your sample app's UI to include a "Login" button or action that triggers the login process. You can use an HTML button element and add an event listener to it that calls the triggerLogin function

# 5. Displaying on the app the id token, access token obtained from authenticating with Passport after login, and the user's nickname

    // Retrieve the ID token
    const idToken = passportInstance.getIdToken();
    
    // Retrieve the access token
    const accessToken = passportInstance.getAccessToken();
    
    // Retrieve the user's nickname
    const userProfile = await passportInstance.getUserInfo();
    const nickname = userProfile.nickname;
    
    console.log("ID Token:", idToken);
    console.log("Access Token:", accessToken);
    console.log("Nickname:", nickname);

# 6. Logging out a user
    
    async function logoutUser() {
      try {
        // Call the logout function to log the user out
        await passportInstance.logout();
    
        // After logging out, you can redirect the user to a specific page or display a message
        // For example, you can use window.location.href to redirect to a thank you page
        window.location.href = 'http://localhost:3000/thank-you'; // Replace with your desired URL
      } catch (error) {
        console.error('Error logging out:', error);
      }
    }
    
# 7. Initiate a transaction from Passport, such as sending a placeholder string and obtaining the transaction hash

    async function initiateTransaction(toAddress, data, value) {
      try {
        // Prepare the transaction object
        const transaction = {
          to: toAddress,
          data: data,
          value: value
        };
    
        // Send the transaction request
        const transactionHash = await provider.request({
          method: 'eth_sendTransaction',
          params: [transaction]
        });
    
        console.log('Transaction Hash:', transactionHash);
      } catch (error) {
        console.error('Error initiating transaction:', error);
      }
    }
    
    // Usage: Call the function with the recipient address, data, and value
    initiateTransaction('0xRecipientAddress', '0xTransactionData', '0xTransactionValue');

# Conclusion
You've successfully integrated Immutable Passport into your web3 application. Users can now log in, log out, and access their information securely. Continue to explore and customize your application to take full advantage of Immutable Passport's capabilities for user authentication, wallet management, and transactions.
    

