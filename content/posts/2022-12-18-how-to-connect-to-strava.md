---
title: Minimal Example to Connect to Strava API
math: true
---


# The Strava API for newbies
Disclaimer: this is my own take on how to use the strava API and reflects my current understanding of it; for more information see [the official strava API documentation](https://developers.strava.com/docs/).

# Strava API
The Strava API allows an app to access and download data on Strava athletes, segments and activities. The Strava API has the following security features, which will help explain the requirements to connect to it:

* one shouldn't be able to access information on an athlete without their permission
* if an athlete has given their permission to access their data, it should be accessible for a given period of time (not a one time use, but not an indefinite access)

Strava solves these issues using oauth2 authentification procedure, that it:

1. You provide a link to the athlete, which prompts them to give your client (your app) access to their data. The type of access granted, the athlete id and client id are encoded into a code, which is given by strava in an url (which can be made to redirect to your app's webpage, granting the code and therefore the information on the connection). Since the code and client id are public, strava requires on top of this a client secret, which is given to the owner of the app on their strava profile, and which allows strava to use the code to give access to the client id, only if it corresponds to the client secret. This is why it is important to keep the client secret _secret_.
2. To actually get acces to the data, strava requires an access token. These token only provides access for a limited amount of time (at the time of writing this article, 6 hours). To get one, you need the above code, client id, and client secret. Strava checks that everything works out (that the code corresponds to the client id and client secret), and then if everything is OK, it provides an access token and a refresh token. The access token grants access to the data, the refresh can be used on to refresh an expired access token.




## API requests: the basic principle
Information is retrieved from the strava API by requesting a specific URL of the type [https://www.strava.com/api/v3/requests]() (which we call an API request) where "requests" is a string encoding what you want to access to. For instance, to request information on athlete number 1234142, the request could be [https://www.strava.com/api/v3/athlete/1234142](). We then access the information strava stores on this athlete in a neatly pre-processed `.json` file. A `.json` file is just a text file with the `.json` extension that encodes the information in a dictionary-like structure.

Hold on, does strava allow everyone who knows to write an API request to access the private information of any athlete? No, the information is securely locked by an authentication procedure (it is `OAuth2` at the time of this writing, see [the official documentation](https://developers.strava.com/docs/authentication/), so if you write in your browser [https://www.strava.com/api/v3/athlete/1234142]() you will get an error.

To access user information, you need the user themself to provide you with an access token `access_token`, which lookes like `f6036877ca12f5d64dke31278af583d80g87dls`. Among other things, this `access_token` stores 


In principle, you could get your user 

- they need to know which application is using the data (which application is making the request)
- they need the permission of the athlete to have their data accessed

