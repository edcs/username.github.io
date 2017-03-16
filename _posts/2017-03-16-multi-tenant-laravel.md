# Introduction

The popularity of software as a service, or SaaS, applications seems to be on the rise and with that there has been a growing
interest in building multi-tenancy into applications. But hang on, what is multi-tenancy?

Multi-tenancy refers to an architecture style where a single application serves multiple groups of users (tenants). Each group
of users will only have to access their configuration, data and functionality. You may be familiar with this type of
application if you've ever signed up to a service where you're required to create a 'team' or 'organisation'. Examples of
multi-tenant applications include services like Slack where you are sharing their infrastructure with a number of other users
around the world, but you're only able to speak to and see the users in your team.

# Tenant Data Isolation

Security is usually your most important consideration when building any application; with a multi-tenancy you must also 
consider how you're going to isolate your customer's data. It's important that users from other teams or companies cannot see
or access each other's data (unless that's a feature of your application). This is where you have an important decision to 
make, do you want to use resources sparingly and share them or are you happy to  use more resources to isolate your user's 
data?

There are two extreme ends of the decision you need to make here regarding how you store your data. At one end there's 
infrastructure isolation, this is where you'd have persistence infrastructure on a per-tenant basis (i.e. you'd create a new
database for each team who signed up to your application). At the other end you have shared infrastructure where your tenant's
data is all stored in shared infrastructure (i.e. you have a single database where all application data is stored).

You need to decide which end of the spectrum your application needs to sit based on the requirements of your users. If for
instance you're creating an application for the finance industry there might be regulations stipulating that their data cannot
be stored in a multi-tenant environment. Or, you could be building an application where no such requirements exist and you'd
like to take advantage of the cost savings and reduction in complexity of shared infrastructure. In some circumstances you
could even sit in the middle, storing multiple tenant's data sharded across multiple databases so that a tenant's data is
not stored as a complete set in one location.

# Multi-Tenancy In Laravel

My experience with multi-tenant implementations has been exclusively with applications which use shared infrastructure. In a
previous job, I was the lead developer at a company which made electric vehicle chargers. These chargers would be owned by a
group (the tenant) who'd give access to their staff or customers (the users) via a mobile app. The mobile app would
communicate with an API built with Laravel to issue commands to the chargers. The customer data was stored in a single Amazon
RDS database and the database was structured in a way that customers could be queried by their group.
