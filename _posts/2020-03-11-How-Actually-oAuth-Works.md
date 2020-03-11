---
layout: post
title:  "oAuth2 Working Principle"
author: "Cagan"
comments: false

---

OAuth 2.0 is the industry-standard protocol for authorization. Is is also a framework. OAuth 2.0 focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices. 
Oauth is not authentication. It is a authorization. So, what is the difference between Authentication and Authorization? 

### Authentication:
 Authentication is all about validating your credentials like User Name/User ID and password to verify your identity. Authentication is usually done by a username and password, and sometimes in conjunction with factors of authentication, which refers to the various ways to be authenticated. 
    - Single-Factor Authentication: Its the simplest authentication method. It uses just password and username or email.
    - Two-factor Authentication: As the name suggests, it's a two-step verification process which not only requires a username and password, but something only user knows. For example phone code you receive or any authentication applications.
    - Multi-Factor Authentication: It's the most advanced method of authentication which uses two or more levels of security from independent categories of authentication to grant user acces to the system. Usually Financial companies, banks use this system.

So authentication means confirming your own identity.

### Authorization:
It occurs after your identity is successfully authenticated by the system, which ultimately gives you full permission to access the resources such as information, files, databases, funds, locations, almost anything. Authorization determines your ability to access the system adn up to what extent. Once your identity is verified by the system after successful authentication, you are then authorized to access the resource of the system. 
Authorization is the process to determine whether the authenticated user has access to the particular resources. It verifies your rights to grant you access to resources such as information, databases, files, etc. Authorization usually comes after authentication which confirms your privileges to perform. In simple terms, it's like giving someone official permission to do something or anything. It is originally created not for a service to authorize a person, it was meant for service to authorize another service. 

Imagine we have photo printing service and it lets people upload photos to our website and they can order prints of these photos. Well, nobody keeps photos in their machine anymore. Instead they use cloud system. For example users want to be able to store their photos in Google Drive and use it without any download and upload requirements. So, how to we implement this? We need to connect between the user's Google account and our service account and their files. Google Drive uses google authentication  to use it and our application uses our authenticated method, so they are completely seperate each other. How can our application do that the users files on Google Drive need Google authentication. How can we write code for our website that can authenticate with Google on behalf of our users? Here is something we can do. We can ask the user for thier Google ID and password. And our website asks us "Hey user, to achieve your demand I need to connect your Google account, but I can't. So give me your Google username and password, and I can login to your account for you, access your photos and print them. Do you think does this work? I think it work in theory but this doesn't make any sense. Becuase if we do that we give all of the permission of our Google account. But we need to access just photos and its not secure thing. 

So, how do we have a third party service authorize with the Google address book service as your user, without the user providing their credentials? So this is where *oAuth* comes in.To solve this problem of services trying to access each other on behalf of the user was the standard created called oAuth.
There are 2 versions of oAuth. oAuth1.0 and oAuth2.0. oAuth2.0 is most widely used one.

### Parking attendants / valets Example:
> Lets say rich guy drives up to a parking garage and he gets down  and hands his key to the valet and he says "Hey mr. valey guy, can you please park this car?". And the valet takes the key, drives the car and finds a parking spot and parks the car there. Okay but, here is the thing. Rich people drive expensive cars and they ("Not me!"), carry expensive things. So what if a malicious valley takes the car keys and takes the car for a long drive or opens a truck or glove compartment and looks at what's inside? So this is why some cars come with an additional key called the valet key. The valet key is just like the main car key, but with reduced access. This valet key can start and stop the car but can not glove the compartment. So we have restrictions. So lets say we have 2 servies. Car Service and Valet Service. The Valet Service needs access to the Car Service directly to do the job, so rather than give the complete credentials of the Car Service to the Valet Service, rather than the giving master key to the valet service, the car owner gives reduced or limited access to the car service by providing the valet key. This is similar to how  oAuth works. oAuth is an authorization mechanism where service can authorize agains each other on your behalf. Once you have given them permission. By the way, it is an open standart and it needs to be becuase all of the services need to talk each other in order to get job done. There is certain flow that needs to happen for this whole process to work. This is called oAuth flow. Lets, come back our photo print example again.

We have photo print service, google drive service. We have a user who is logged into both your photo printing service and to Google service. Each service trust the user, but they don't trust each other. And we want these two services trust each other so they can comminicate. 
Our print service go to our google drive service and say, "Hey, can I access this users files?". And Google service, says "No, go away!". However, if we have two services that have oAuth implemented google will act different. This time print service says "Hey, I need this users file". This time google does something interesting. It goes to the user and says "Look here man, there is a service that's annoying me saying it wants to access some of your files. Do you trust it?". User sees the screen that says, who is asking for access to that users Google account and what are the list of permissions that service wants. If the user is a person who's trying to print the photo, they will look at this and "okay, this is all correct please allow access." And Google says "Okay, users verified and allow these permisssions, "Okay service, you can have access." And Google gives the service a **key** token called an authorization token. It contains just the allowed permissions. It is a valet key. It's a key with limited access. And printing service says "Ok, cool. I'm gonna save this".And now every time the photo printing service needs to access Google Drive, it just hands this token to the request and says, "I need access to this file" and google control that token and it says "Hmm, I know this token. I gave it to you. Okay, I will give you information but just limited". 
And flow keeps continue until token expires or deleted.















