---
author: "Naveen Honest Raj"
date: 2017-12-15
title: Writing your first Business Model in Hyperledger Composer
tags: [
    "hyperledger",
    "composer",
    "fabric",
    "blockchain"
]
weight: 10
description: This article focuses on creating a simple business model that helps you with visualizing the abstract nature of Hyperledger Composer’s language.

---

This article focuses on creating a simple business model that helps you with visualizing the abstract nature of Hyperledger Composer’s language.

If you are confused on where to start or wasted a lot of your time in getting started with Hyperledger Composer, you should read this article. My co-worker, Varun has explained it in such a simple way that anyone can understand it.

## What is Business Network Definition?
The Business Network Definition is the core definition that holds the Hyperledger Composer’s programming model. In short, it controls the model definition, the relationship between them, the access control over them and the actions that could be performed on/with them.

BND has three core components : 

1. The Model written in .cto files
2. The Business Logic written in .js files
3. The Access Control Logic written in .acl files

(Optional) There is a Query file written in .qry files that help us to query over the persistent DB.

These three components are easy to maintain and they govern our whole business application on the Composer. 

Let’s see how to write a manageable simple model file that helps anyone understand most of our business logic. We will implement this in the business logic file.


## Understanding The Key Concepts Of A Model
Before writing our own model, let us understand the key concepts of it. In a model file, you will be defining the following, 


1. A namespace
    namespace org.skcript.svr
2. Resources, which includes
  1. Participants
  2. Assets
  3. Transactions
  4. Events
3. Optional import statements that are used to inherit or import other model files.


## Resources Are Everything In A CTO File
A ‘Participant’ denotes the person who does the action (in simple terms). This is to whom the ID is issued and this is the one who performs transactions.

For example: Consider the example


    participant Author identified by id {
        o Integer id
        o String name
        o String email
        o String specialized_in 
        o Double age
    }

In the above example, we see there’s a participant named `Author` and the keyword `identified by` is used to select the primary key. Here, it is `id` field that acts as a primary key. We will know the wide applications of having this `identified by` on right field to have an easy and readable access to the model.

The circles are nothing but lower case ‘o’ (The O, yeah ! ).  Composer’s model file has a unique way of representing a new key in their object. But you will get used to it.

Assets are the resource which our business model has. Considering our above example, `Author` is a participant, then `Post` will be the asset that he holds. And we know that every post will always belong to an author. There can never be an asset that won’t belong to an author. This can be done by creating a relationship between our participant and asset. This is easy-peasy because of the CTO’s modeling language. 

P.S. You should be careful with relationship because it is unidirectional and you can’t do the vice versa which you can do in most of the application frameworks.

Example:

    /**
    * An enumerated type
    */
    enum Category {
      o AI
      o BLOCKCHAIN
      o WEB-DEVELOPMENT
      o BACKEND
      o DESIGN
      o FUN
    }
    
    /**
     * A post asset.
     */
    asset Post identified by id {
      o String id
      o String title
      o Category category
      o DateTime timestamp
      --> Author author
      --> Author coAuthor optional
    }

In the above example, as we learned earlier `identified by` is used with `id` as primary key which makes sense. Right? Now you can understand.

But where’s the new thing coming up? No rush. Let’s take it slow. 
Let’s see the `Line 21` - we see an arrow. Okay. I didn’t write it because it looks good on the code and it’s not a comment too. That’s again our Composer’s modeling language’s weird way of writing relationship. So we should read it as Post belongs to Author. That’s simple. Done.
In `Line 22` , we can see there’s a new keyword `optional`. This helps us avoid mandatory entry to create an asset. This is defined as optional by thinking that there may or may not be a coauthor for a particular post.

Next, why `enum` ? It’s for managing a neat code. As our articles goal is not only to learn how to write a model but also to write a good one that can be easy to read and update.
So enum holds the possible values our field can take. So we defined an `enum` called `Category` and we gave it all the possible values it can take. Now in `Line 19` we used it and created a category of `enum Category` . 

Transactions are the process in which our participants do the operation on assets. Considering our scenario, author creating a post is also a transaction. 
Example:

    transaction createPost {
      --> Author owner
      o String title
      o Category category
    }

The above transaction definition emphasizes that we are creating a post by pointing out the author of the post. We also provide title and category while creating the post.

P.S: In our next post, we will learn more about the transactions and transaction processor functions. 

Events are used to trigger some other third party application. You can write the logic for the event which will help you to send an email about the author’s new post. These events can be used to help you to send emails if and only if you subscribed to the particular author which helps to avoid spam and improves integrity. Events are also defined the same as transactions. The purpose is only different. Used with `event` keyword for declaration.


## The Endless Possibilities Of The CTO File
You can still segregate the model declaration by `abstract`  keyword which won’t create participant or asset in the registry, rather is used to inherit from the other resources.

Example: Considering the same scenario, try to understand the following code


    abstract Participant UserInformation identified by name {
      o String name
      o String address
      o String email
      o Integer age
    }
    
    participant Author identified by id {
        o Integer id
        o String specialized_in 
        o UserInformation user_information
    }

We created an `abstract` participant which holds most information about a user and we inherit and use that in our `Author` participant. This helps us have a clean model that still maintains Single Responsibility Principle. We can do abstract type for assets and transactions too. 

## Conclusion
There are much more magical things like `extends` that we can do with CTO file. But let’s end it clean and easy here. After you write your first model, you will figure out most of the conventions you might need to follow to have a maintainable code. Wish you a good luck, readers.

If you have any difficulties in writing your own model, feel free to email me at naveen@skcript.com. 



