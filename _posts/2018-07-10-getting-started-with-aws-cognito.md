---
title: "Getting Started With AWS Cognito"
layout: post
date: 2018-07-10 15:45
tag:
- aws
- cognito
- idaas
- auth0
- okta
- serverless
- lambda
blog: true
author: edcs
description: A tale of my first AWS Cognito experience.
---

So you’ve decided that you want to use an ‘ID as a Service’ provider to secure your mobile or web app and 
corresponding APIs and services. You’ve looked at services like [Auth0](https://auth0.com/) and 
[Okta](https://www.okta.com/) but decided they’re not what your looking for and you’ve then found 
[Amazon Cognito](https://aws.amazon.com/cognito/)? 

Well, that’s at least my story; services such as Auth0 and Okta are feature rich and are supported by excellent 
documentation, but that sort of product doesn’t come for free. Their subscriptions usually entice you in with a free 
tier, but once you start to scale, the costs can become a bit prohibitive (especially if you're looking to use
'enterprise' features such as SAML). 

This is where Cognito comes in - it offers similar features but at a lower cost but with what’s arguably a poorer 
ecosystem of supporting libraries and documentation. This is where you need to make a trade off between getting your 
hands dirty with Cognito or paying for a ready made solution.

## Why Use ID as a Service?

I’m not a security expert and I think that’s the point here. In the past I’ve created ReSTful web services secured with 
[PHP OAuth 2.0 Server](https://github.com/thephpleague/oauth2-server) and it was a steep learning curve. The world of 
computer security is fast moving and ever evolving - it doesn’t matter how smart you think you are, there’s always 
someone else smarter! So when a service becomes available which offers to solve a complex problem like security for a 
low cost, I’m interested.

Cognito is feature rich offering traditional username and password sign-up/in, social sign-up/in, ‘password-less’ and 
enterprise sign-up/in using SAML or OpenID. Then on top of that; you can enable multi factor authentication, SMS 
notifications, it has customisable hosted sign-in/up pages, and they’ll even handle some of the transactional emails 
you’ll undoubtedly need to send to your users when they need to do anything like reset their password.

Imagine trying to build all that yourself!

## I’m Sold, What Now?

You may have already signed up to AWS and taken a look at the Cognito console. This was my first ‘what the…’ moment, 
you’re asked whether you want to create a ‘User Pool’ or an ‘Identity Pool’; what does this even mean?

[![Cognito Interface](/assets/img/getting-started-with-aws-cognito/cognito.png)](/assets/img/getting-started-with-aws-cognito/cognito.png)

### User Pools and Identity Pools

A ‘User Pool’ is a complete user directory for your application. ‘User Pools’ provide the necessary functionality
required for the complete user lifecycle. Processes such as sign-up, sign-in and sign-out are all covered using a handy
API and ready made UIs (which can be customised with your own branding). Once a user is authenticated via ‘User Pool’, 
they are issued a [JSON Web Token](https://jwt.io/) - you’re then responsible for validating the JWT and ensuring 
whether the user should have access to your application.

By contrast, an ‘Identity Pool’ issues temporary AWS credentials to users who are authenticated via a third party 
identity provider. An identity provider could be Google, Facebook, your corporate SSO system (using SAML/OpenID) or 
interestingly, Cognito. Roles and permissions for credentials issued by an ‘Identity Pool’ are managed by 
[Identity and Access Management](https://aws.amazon.com/iam/).

So, if you’re looking for a way to securely create a user directory which will allow you to authenticate users against 
any application which can validate a JWT, ‘User Pools’ are for you. However, if you want your users to directly interact 
with AWS services then you’ll need to use ‘Identity Pools’.

### A Note About AWS Amplify

[AWS Amplify](https://aws.github.io/aws-amplify/) is a JavaScript library provided by AWS which simplifies many common 
interactions with their services. If you’ve been hunting around the web trying to figure out how best to implement 
Cognito, you may have come across it.

The project I was working on which sparked my interest in Cognito required a React web front-end and a React Native 
mobile app. The front-ends would then interact with a [Serverless](https://serverless.com/) based backend. To make
things interesting, my client was keen to use G Suite as a the identity provider. Great, I thought; Cognito does 
all of this!

AWS Amplify combines ‘User Pools’ and ‘Identity Pools’ to give users direct access to your AWS services. Development for 
the web front-end was quick; I didn’t need to worry about authenticating users against API Gateway as AWS Amplify did it 
all for me 🎉

I usually get a nagging feeling when this sort of thing starts happening - I’m relying on something which I don’t fully 
understand and it’s performing lots of magic in the background. It wasn’t much time until I found out that SAML 
authentication was unsupported on React Native by AWS Amplify. At this point I was almost ready to give up, however
after some research and source diving I figured out what was going on.

I found out that the difference between ‘User Pools’ and ‘Identity Pools’ was really important. I didn’t need a ‘backend 
as a service’; I was building an API which would operate as a layer on top of the underlying AWS services being used, so 
having users interact directly with API Gateway using IAM credentials was totally unnecessary. If Cognito could just 
issue the users with a JWT, I could use a custom 
[API Gateway authoriser](https://serverless.com/framework/docs/providers/aws/events/apigateway/#http-endpoints-with-custom-authorizers) 
to verify it’s validity and allow or deny access.

I updated my Cognito service to solely use ‘User Pools’ and found that I could use any flavour of compliant OpenID 
Connect client library. For the React Native mobile app I used 
[react-native-app-auth](https://github.com/FormidableLabs/react-native-app-auth) and I just rolled my own for the web 
front-end.

_(I’ll cover the above topics in more detail in future articles)._

## Summary

To sum things up, Cognito is a great service for authenticating users and maintaining a user directory. It also has the 
capability of issuing temporary credentials for directly accessing AWS services.

If you’d like to provide authenticated access to an API or application which is capable of verifying JSON Web Tokens 
then you should consider using Cognito ‘User Pools’. You’ll have to verify the JWT yourself and maintain your own roles
and permissions system if required.

If you’d like to give your users direct access to AWS services then you should consider using ‘Identity Pools’. 
‘Identity Pools’ give temporary credentials to users that have been authenticated against a third party, this can 
include an existing Cognito ‘User Pool’. You may find that AWS Amplify is helpful for this type of application.

_I hope you found this article useful. If you have any issues with it, feel free to 
[report](https://github.com/edcs/edcs.github.io/issues) them or [fix](https://github.com/edcs/edcs.github.io/pulls) 
them 👍_

