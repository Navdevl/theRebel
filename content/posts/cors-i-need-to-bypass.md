---
author: "Naveen Honest Raj"
date: 2018-03-10
title: CORS bypass - Funny trick to get through the same-origin-header 👻
tags: [
    "cors",
    "trick",
    "origin",
    "hack"
]
weight: 10
description: This article explains CORS basics and demonstrates a way to get through few CORS api calls.
---

# CORS - What and Why ?

As per **Mozilla's definition**, 

> Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to let a user agent gain permission to access selected resources from a server on a different origin (domain) than the site currently in use.

It's little technical to understand, right? Let me clue you in with a nice example.

You are sending a fake-letter to a person *'X'* using someone's identity. This 'X' is a genius so he figured out that it was not from the person it is identified as by seeing the signature and the way the letter was sealed. Because he knew that the person used to seal and pin a star sticker to it. This was missing and so 'X' rejected the letter.

Exactly, this is our CORS does. It will help us to figure out where the request are sent from.

# A Technical Scenerio
Let us consider that there is a blogging website *example.com*. People can write blog posts and like other people blog post. This site also serves an API because there is an mobile app to the site.
So when our friend 'MrBlue' debugged the website's requests, he saw that whenever he likes other people's post, the request looks like this

> **POST example.com/post/post_id/like**

And he figured out that there are no CORS-headers passed to it. I told you right. CORS headers are like signatures passed. There will be a preflight call happening before the normal call to make sure of the origin. So it wasn't happening in our example.com blog. 

**What was exactly happening, then?** Everytime when the request was handled, the post's like was increased by using the Cookie that identifies our user.

So, 'MrBlue' wrote a website and whenever someone visits this page, it just makes the post call from his website because example.com doesn't check for origin-policies neither it has CORS enabled.

> **POST example.com/post/post_id/like**

and gets likes to his post. All he has to do is forward his link to the people whose cookies are in the browser i.e, the people who are logged in. 


# How do I tackle those scenerios ?

Enabling CORS to your web application will always send a preflight-request before every api request. If this preflight request fails, your regualar API call won't take place. This restricts API call happening from any other domains other than the domain it was hosted.

Enabling CORS to your web application differs from application to application.

For a simple Python Flask server, it is enabled by the following snippet.

```
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app)
```

# Our little hack to bypass CORS enabled sites

There is this reverse-proxy library that gives you a way to make API calls. 

[Cors Anywhere](https://github.com/Rob--W/cors-anywhere) is a NodeJS proxy which adds CORS headers to the proxied request. You can read more about it's working and architecture in the repository.

I am here to give a clean-small-example to see its working.

Normally if I wanted to get all posts by user "MrBlue", I need to ping

> **POST example.com/user/mrblue/posts**

But now example.com has enabled CORS so I can't get the user's post to be displayed on my site. But I wanted it to be done. 
I wrote a jquery method that get users post from example.com

The normal approach.
```
function getTasks() {
  $.ajax({
      type: "GET",
      dataType: "json",
      crossDomain: true,
      beforeSend: setHeader,
      withCredentials: true,
      url: 'https://example.com/user/mrblue/posts', 
      error: function (data) {
        console.log("Error. Really?")
      },
      success: function (data) {
        // Actions to be performed with the data
      }
    }
  );
}
```

Now when I tried to list the user's post. All I could get is an error. And when I debugged the error, I could see there was a preflight error in console. 

![Preflight Error](/images/preflight-error.png "Preflight Error")
I think you might have to open the image in a new tab to view. It's very small right? 


So now I **Cors-Anywhere-ed** it. And just changing one line of the code gets it done.

```
function getTasks() {
  $.ajax({
      type: "GET",
      dataType: "json",
      crossDomain: true,
      beforeSend: setHeader,
      withCredentials: true,
      url: 'https://cors-anywhere.herokuapp.com/https://example.com/user/mrblue/posts', 
      error: function (data) {
        console.log("Error. Really?")
      },
      success: function (data) {
        // Actions to be performed with the data
      }
    }
  );
}
```

I prepended the URL with "https://cors-anywhere.herokuapp.com/". This is cors-anywhere server running in a public domain. You can also clone the repo and run it locally and make some modification to the reverse proxy server as your wish. 

Now when I tried to list the user's post, the response was success and it got displayed. Simple right? Hurraayyy.. 

# Conclusion
Now I need to figure out a way to steal cookie and to pass it along with the request because default CORS-Anywhere's behavior doesn't sends cookies. If anyout out there did passed the cookie already with this approach, you can comment below. It would be helpful for me as well as for other guys.