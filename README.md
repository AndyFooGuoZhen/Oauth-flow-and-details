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

# Oauth flow visualized
![image](https://github.com/AndyFooGuoZhen/Oauth-flow-and-details/assets/77149531/7e0a934d-8a94-4b5a-b54a-60808e0c500e)

In this example, after successful auth, a code will be retrieved from google, yelp will then use this code to exchange an access token from google to access the contacts api. This access token limits the things that Yelp can access but may also have a time limit as to how long this access token is valid. 
