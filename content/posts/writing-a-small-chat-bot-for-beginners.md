---
author: "Naveen Honest Raj"
date: 2018-03-01
title: A Beginner's journey - Writing a Chatbot  ⭐
tags: [
    "python",
    "chatbot",
    "beginner",
    "wit.ai"
]
weight: 10
description: This is a beginner's article that helps to get an idea on getting started with chatbots.
---

# Introduction

**Chatbots** are over-hyped nowadays. Building a chatbot isn't a big deal. But it's not going to be-like-human at all. Because we can answer all the queries, but our chatbot wont be doing that. Maybe in future you might do build one that can answer everything, but in our article, we are going to build a bot that is an expert in stock. Yeah.! A bot that can excel very well with stock informations.

### What our Chatbot is NOT going to do
It's not going to connect itself to internet and learn everything. It's not going to happen like this.

> **Me:** Learn about stocks.</p>
> **Bot:** Connect me to the fastest internet. Maybe try Jio.

That's not how are bot is going to work. We have to learn the knowledge before going to feed it the knowledge-base. It means, if you wanted to build a bot to answer your stock-related-queries, you should feed it with those particular information.

### What should I need to code a Bot

A **small brain** to read and understand few libraries documentation.
If you dont know **Python**, you might need few **minutes of your time** to learn the basics. 
**Aaaannndddd...** I guess that's all.

- [x] A smart brain
- [x] Basics of Python Programming Language
- [x] Time that you spent on.. Uhm.. Nothing 😋

# Let's get started

Before going into the programming part, we are going to learn two things.

1. Intent Classification
2. Entity Extraction

This two will do most of our works. So what is intent classification.

### Intent Classification
When I say "Hey, Good morning", it is categorized as an intent that belongs to **GREETINGS**. But when I say "Bruh, What's the time ?", it is an intent of **QUESTION**. Each intent has a different way of responding. While greeting, you don't need much of a data to respond back but for a question you will need data related to the context. 

### Entity Extraction
"What is the time in Canada, dude?" will be crushed down into meaningful entities like "asking for time", "time in Canada". In simpler terms it can be written as:

> **Action:** GetTime

> **Location:** Canada

### Whats Next ?
But how will I teach my bot to understand the intent and extract the entities? The answer is you should feed it with a lots of data and grammar knowledge. For example:

> Hey = Greeting

> Hello = Greeting

> Wassup? = Question

> Time? = Question

> Good luck = Greeting

and sooooo on.. Boring and tiring right? And that's why there are some companies out there who made these knowledge into engines and help us to build on them.

Dialogflow( formerly api.ai ), Wit.ai, LUIS are few among them. And today we are going to use **WIT.AI**

# Getting started with Wit.ai 

First, you have to sign up and create a app. Your app interface will look like this.

![Wit.ai Introduction](/images/wit-ai-intro.png "Wit.ai Introduction")


Now write a sentence in the "User says"-form and select the entity you wanted to select and give it a search-query-label.

It will look like this after selection.
![Wit.ai Entity Selection](/images/wit-ai-entity-selection.png "Wit.ai Entity Selection")
<br>
You can add roles i.e, extra information to your search query, say- organization by clicking on the cirle over there.
![Wit.ai Adding Roles](/images/wit-ai-adding-roles.png "Wit.ai Adding Roles")
<br>
**Remember this carefully** You need to select before giving an entity-label. But for intent-classifying, dont select anything. Just give an intent, say "Question"

![Wit.ai Intent](/images/wit-ai-intent.png "Wit.ai Intent")

Train the system with as many possible values. This will improve the system to understand more.

**Example:**

> Stock market price of Apple

> Apple's stock value

> Stock price of Apple

> What is apple's stock price

and so on..

Now you can see the result in "Machine-feedable-format" i.e, JSON. Click on the small graph-like-icon over the right side. It will show you how the response looks like. If you feed more data to your system, it will understand perfectly.
![Wit.ai Json Response](/images/wit-ai-json-response.png "Wit.ai Json Response")

# Finale with Wit.ai
Go to settings over the top. Click over there. Generate the client token and have it somewhere safe. You will need it sooner. Now we are going to the CODING PART.
![Wit.ai Client Token](/images/wit-ai-client-token.png "Wit.ai Client Token")

# Programming is Power
Let's get started with a **Devil's laugh** <p>
**So what do you need exactly?**

1. You should have a decent code-editor like Sublime - My favorite ;) or Atom or VScode etc.
2. Now install Python 2.7 in your system. Refer [this article](https://www.howtogeek.com/197947/how-to-install-python-on-windows/). Windows users, don't forget to add PYTHON to your PATH.
3. Install pip. So whats pip? Refer [this article](http://www.pythonforbeginners.com/basics/python-pip-usage) for more information
4. Now go to your terminal/console/command-promt. And type `pip install wit`
5. Go learn few python basics from [LearnPython](https://www.learnpython.org/). You should know how to use `dict()` type in Python. That's what we need the most. 

That's all. Our environment is set.

# Code it dude
Below is the **simplest-code** which will ping your application in Wit.ai. I have added few comments for the understanding. Later I will explain the important lines needed for understanding.

```
# Simple Code - Makes No Sense. Only for Beginners.
from wit import Wit
ACCESS_TOKEN = "CY4DTYXXCOGEIJLCNW7CI3QSXXXXXXXX"

def get_stock_price(organization):
  # This is going to be a dummy function that returns a constant value now. Rewrite with your smart-brain
  return 1024

client = Wit(ACCESS_TOKEN) # This object will helps us to communicate with our wit.ai app

user_input = raw_input('>> ')
resp = client.message(user_input)

entities = resp["entities"]
if entities["intent"][0]["value"] == "Question":
  organization = entities["organization"]["value"] # Edge cases are not handled
  stock_price = get_stock_price(organization) # This is an imaginary function where you should write definitions that will fetch the stock price. Learnt it yourself dude.
else:
  print("Sorry. I can't understand")
  exit(0)

print("The stock price of {0} is {1}".format(organization, stock_price))

```


The response for `resp = client.message(user_input)` will look like this.

```
  {
    "confidence": null,
    "intent": "default_intent",
    "_text": "Stock price of apple",
    "entities": {
      "organization": [
        {
          "suggested": true,
          "confidence": 0.89801424426314,
          "value": "apple",
          "type": "value"
        }
      ],
      "intent": [
        {
          "confidence": 1,
          "value": "Question"
        }
      ]
    }
  }
```


# Conclusion
The more the intents, the wider the scope of your chatbot will be. The more cases you handle, the more interactive it will be. You can later integrate it with any UI and run it. Enjoy 🎉 🎉 🎉

**If you have any doubts on any part of this execution, feel free ping me at naveen@skcript.com.**