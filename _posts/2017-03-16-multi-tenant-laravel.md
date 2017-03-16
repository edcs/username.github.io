# Introduction

The popluarity of software as a service, or SaaS, applications seems to be on the rise and with that there has been a growing
interest in building multi-tenancy into applications. But hang on, what is multi-tenancy?

Multi-tenancy refers to an architechture style where a single application serves multiple groups of users (tenants). Each group
of users will only have to access their configuration, data and functionality. You may be familiar with this type of
application if you've ever signed up to a service where you're required to create a 'team' or 'organisation'. Examples of
multi-tenant applications include services like Slack where you are sharing their infrastructure with a number of other users
around the world, but you're only able to speak to and see the users in your team.

# Tenant Data Isolation

Security is usually your most important consideration when building any application; with a multi-tenancy, you must also 
consider how to ensure data isolation too. It's important that users from other teams or companies cannot see or access each 
other's data (unless that's a feature of your application). This is where you have an important decision to make, do you want
to use resources sparingly and share them or are you happy to  use more resources to isolate your user's data?
