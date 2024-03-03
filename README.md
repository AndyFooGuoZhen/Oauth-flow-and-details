# Resource : https://www.youtube.com/watch?v=996OiexHze0

# Old ways of getting user info
Application asks for user's credentials, use user's crentials to login to their account , get information and logout, making a "promise" not to touch their personal info. This is incredibly risky and not a good way of accessing user's info. Example , yelp wants to have access to gmail contacts, it uses my gmail and password to login and get those information from my gmail. In this case, you are trusting yelp to only access information that's needed.

# Solution: Delegated auth with Oauth 2.0 (Oauth Flow)
Suppose yelp has a connect with google button, user's click it and is redirected to google login. Google prompt appears to ask user if they want to allow yelp to access contact information. If yes, the user will be redirected to yelp's page again. In this case, you are trusting google to handle information access.

# Oauth terms
- Resource owner - The user who can say yes to the prompt, user who owns the data (My google info in google)
- Client - The application (Yelp)
- Auth server - System that I can use to say yes (accounts.google.com)
- Resource server - Api that holds the data that the client wants to get to (google contacts api)
- Auth grant - Proof to say that the user has said yes
- Redirect URI - Path to redirect back after the flow is donw
- Access token - Key that client uses to access information that is granted (Yelp uses this to access information in google contacts api)
- Scope - Auth server has a list of scopes it understands (EX: contacts.read, contacts.write..) , client uses this to tell auth server what kind of permissions is needed

# Oauth flow visualized (Authorization code method)
![image](https://github.com/AndyFooGuoZhen/Oauth-flow-and-details/assets/77149531/7e0a934d-8a94-4b5a-b54a-60808e0c500e)

In this example, after successful auth, a code will be retrieved from google, yelp will then use this code to exchange an access token from google to access the contacts api. This access token limits the things that Yelp can access but may also have a time limit as to how long this access token is valid. 

# Back channel and front channel

## Back channel (highly secure channel)
Server code, no one has access to but only the developer. Exchanging auth code with access code is handled in the back channel. Communication between the resource server (EX: where we get our contact information) is also handled here.

## Front channel (secure channel)
Browsers, less secure. Password stored on code can be exposed via inspect elements, network calls. Things like redirect uri, code, auth code (retrieved after successful auth from auth server) can be exposed.

# Why use exchanging of code for access token
To make it more secure, the exchange of code and access key is handled by the backend server (Back channel). After successful authorization from the auth server , the user is redirected back to the client application along with the auth code which will then be posted to the backend application. The backend server passes the auth code (retrieved from client application) along with  a secret key (client id and client secret) to communicate with okta (or google auth server using the example) . Once the access token has been retrieved from okta, the backend application can then retrieve information from the resource server using the access token.

# Development steps

## Setup of client id and secret 
We do this so that our backend application can use these credentials to comminicate with our auth server. To do so, we go to the auth server and generate a client id and client secret. 

## Initiate post request from frontend application to auth server
The frontend application then establishes communication with the auth server to retrieve the auth code after successful authorization. Here we specify the scopes, redirect uri.

## Redirect
After successful auth, we will be redirected back to the application via some form of callback redirect uri, and we can retrieve the auth code via querying the uri.

## Initiate communication with auth server for access token
Our backend application retireves the auth code from the client side and incorporates client id and client secret to post a request to auth sever on via a token url. We will then have an access token (that will be vaklid for a certain amount of time).
<img width="670" alt="image" src="https://github.com/AndyFooGuoZhen/Oauth-flow-and-details/assets/77149531/be642ab2-7c78-44d1-9a75-d9a02bd5e06b">

## Getting information from resource server
After having our access code. We can then make a get request and specify a header and pass our access token in as a Bearer.
<img width="661" alt="image" src="https://github.com/AndyFooGuoZhen/Oauth-flow-and-details/assets/77149531/30f3dbf4-98d0-41f8-918d-02ad37753fb5">

# Types of OAuth flow
- Authorization code (Front channel and back channel)
- Implicit (front channel only), no exchange step here
- Resource owner password credentials (back channel only), no exchange, just get access token
- Client credentials (backchannel only)

# Oauth implicit flow
<img width="721" alt="image" src="https://github.com/AndyFooGuoZhen/Oauth-flow-and-details/assets/77149531/40715a8d-86cb-41e4-9312-54640503e5a4">





