# APIs for Beginners

I'm a software developer in training (and in fact) at Turing School of Software & Design. You might not know it, but teaching others is one of the best ways to learn something yourself--and you bring people along with you in the process. Towards that end, as I'm learning about APIs I'll write this guide. I hope it benefits you and inspires you to share your own learning in some way with others!

## What Is an API?

API stands for Application Programming Interface, but that's not very helpful. In practice, an API is an outward-facing layer surrounding an application that receives a request, processes it internally, and sends back a response. Any API has certain expectations about the format of the request which need to be followed for it to be able to process it and respond.

In the context of web development, most APIs are on remote servers (computers running elsewhere that you can communicate with through the internet using your browser), which means the requests and responses will be **HTTP requests & responses**. Unlike a website (which could be described as a kind of API) which sends your browser human-friendly **HTML** in response to your HTTP request (i.e., entering a **URL** into your browser), APIs send the content of their responses in the **JSON** (JavaScript Object Notation) data format which is easier to applications to interact with. They also tend to be **RESTful**, meaning that they deal in resources which are organized in a particular conventional way that makes it easy for others to interact with those resources.

## What Are APIs Good For?

Web-based APIs come in at least two varieties: third-party APIs, and (first-party) microservices.

### Third-Party APIs

Taken singly, any given website only has a certain amount of data and functionality available to it--whatever functionality has been programmed by that developer or company, and whatever data has been entered by the developers and their users. APIs allow developers to share data and functionality with other programs. If you've ever logged into a website using your Google or Facebook account, for example, you've benefitted from an API. That website sent a request to a **third-party API** on Google's servers and received a JSON response containing information about you that the website used to streamline the process of making you an account. This is just scratching the surface of what third-party APIs make possible. See [this Github project which lists publicly available APIs](https://github.com/public-apis/public-apis) and consider what a website could do with the information they make available.

### First-Party Microservices

In the world of software development, object-oriented programming (OOP) is the primary paradigm. OOP design princioples encourages developers to write clean, reusable code that is organized in such a way that components have a single responsibility and are as independent of one another as possible so that it's easy to fix, extend, or reuse elsewhere. This can be taken to the extent of separating out entire pieces of functionality from a main application and letting the main application communicate with it as an API: this is a **microservice**. One of the benefits of this approach is that others can make use of that API for reasons that go beyond its original purpose in reference to the main application; most likely in a monetized fashion, but perhaps freely available within limits.

## How Do I Use an API?

Because APIs send and receive HTTP requests, you can access them the same way you use websites: through your browser. But because APIs send their responses in JSON, and your browser is optimized for displaying HTML, it's not the best way to try out an API. See, for example, what Github's API sends you when [sending a request to view my profile info](https://api.github.com/users/danieleframpton).

```JSON
{
  "login": "DanielEFrampton",
  "id": 40702808,
  "node_id": "MDQ6VXNlcjQwNzAyODA4",
  "avatar_url": "https://avatars1.githubusercontent.com/u/40702808?v=4",
  "gravatar_id": "",
  "url": "https://api.github.com/users/DanielEFrampton",
  "html_url": "https://github.com/DanielEFrampton",
  "followers_url": "https://api.github.com/users/DanielEFrampton/followers",
  "following_url": "https://api.github.com/users/DanielEFrampton/following{/other_user}",
  "gists_url": "https://api.github.com/users/DanielEFrampton/gists{/gist_id}",
  "starred_url": "https://api.github.com/users/DanielEFrampton/starred{/owner}{/repo}",
  "subscriptions_url": "https://api.github.com/users/DanielEFrampton/subscriptions",
  "organizations_url": "https://api.github.com/users/DanielEFrampton/orgs",
  "repos_url": "https://api.github.com/users/DanielEFrampton/repos",
  "events_url": "https://api.github.com/users/DanielEFrampton/events{/privacy}",
  "received_events_url": "https://api.github.com/users/DanielEFrampton/received_events",
  "type": "User",
  "site_admin": false,
  "name": "Daniel Frampton",
  "company": null,
  "blog": "danieleframpton.github.io",
  "location": "Broomfield, CO",
  "email": null,
  "hireable": true,
  "bio": "Systems-oriented critical thinker with excellent written and verbal communication, excited by opportunities to problem-solve and help connect people using tech.",
  "public_repos": 44,
  "public_gists": 6,
  "followers": 4,
  "following": 2,
  "created_at": "2018-06-29T20:04:02Z",
  "updated_at": "2020-01-11T16:19:26Z"
}
```

As you can see in this example, JSON data comes in comma-separated `"key": "value"` pairs surrounded by curly braces (i.e., the "hash" data type). For example, after the opening curly brace, the key `login` is associated with the value `DanielEFrampton`, which is separated from the next key/value pair (`"id": 40702808`) by a comma. All of the keys are double-quoted (i.e., the "string" data type), but the data type of the values can vary; some are strings, some are whole numbers (i.e., "integer" data type), and others are `true`, `false`, or `null` (i.e., the "boolean" data type).

For a more complex example, see the data returned when you [access the "repos_url" for my profile](https://api.github.com/users/DanielEFrampton/repos). A series of hashes is returned within square brackets (`[]`, the "array" data type), and some of the keys have entire ("nested") hashes as their associated value, like a Matroyshka doll. While these are hard but not impossible to understand visually, these tree-like structures are intuitive to navigate using code.

If you'd like a better format for testing APIs, the [free Postman application](https://www.getpostman.com/) is your ticket. In the end, though, to actually make use of APIs you want to be able to use them from within your application.

## Consuming an API within Another Application

Each language and development framework has its own popular plug-ins and libraries for "consuming" APIs. The language and framework I use is Ruby on Rails, so I'll use that to illustrate what consuming an API from within another application looks like.

Ruby has a built-in library called Net:HTTP which the popular **gems** (freely shared Ruby libraries which extend the functionality of Ruby and Rails) Faraday and Httparty gems are built on top of. In this example we'll use Faraday, but they work similarly. This walkthrough assumes you have installed Ruby and have a basic development environment in place, but if you have not and want to follow along, [here are some instructions from Turing's curriculum](https://github.com/turingschool-examples/backend_module_0_capstone#environment).

 - Create a file with the Ruby extension, `.rb`, in a directory of your choice.
 - From your terminal, in the same working directory as that file, install the Faraday gem: `gem install faraday`.
 - At the top of your file, require (i.e., tell Ruby to include the code from a library) the Faraday gem: `require 'faraday`.
 - You can now use Faraday methods to send a request to an API and store a response.

To create a request, use the Faraday class method `get` with the URL as the argument and store the return value in a local variable. E.g.,

  ```Ruby
  response = Faraday.get 'https://api.github.com/users/DanielEFrampton'
  ```

At this point the `response` variable is storing a Faraday::Response object with all the HTTP response data. (For more about HTTP requests and responses, see [this Turing lesson](https://backend.turing.io/module2/lessons/request_response_anatomy).) To extract the JSON data from that HTTP request, you can use the #body instance method available to Response objects and store the return value in another variable. E.g.,

  ```Ruby
  body = response.body
  ```

The `body` variable now contains the JSON data. But, unless it is formatted otherwise by Postman or a browser as it was in the above example, JSON data is transmitted as one long string:
```Ruby
"{\"login\":\"DanielEFrampton\",\"id\":40702808,\"node_id\":\"MDQ6VXNlcjQwNzAyODA4\",\"avatar_url\":\"https://avatars1.githubusercontent.com/u/40702808?v=4\",\"gravatar_id\":\"\",\"url\":\"https://api.github.com/users/DanielEFrampton\",\"html_url\":\"https://github.com/DanielEFrampton\",\"followers_url\":\"https://api.github.com/users/DanielEFrampton/followers\",\"following_url\":\"https://api.github.com/users/DanielEFrampton/following{/other_user}\",\"gists_url\":\"https://api.github.com/users/DanielEFrampton/gists{/gist_id}\",\"starred_url\":\"https://api.github.com/users/DanielEFrampton/starred{/owner}{/repo}\",\"subscriptions_url\":\"https://api.github.com/users/DanielEFrampton/subscriptions\",\"organizations_url\":\"https://api.github.com/users/DanielEFrampton/orgs\",\"repos_url\":\"https://api.github.com/users/DanielEFrampton/repos\",\"events_url\":\"https://api.github.com/users/DanielEFrampton/events{/privacy}\",\"received_events_url\":\"https://api.github.com/users/DanielEFrampton/received_events\",\"type\":\"User\",\"site_admin\":false,\"name\":\"Daniel Frampton\",\"company\":null,\"blog\":\"danieleframpton.github.io\",\"location\":\"Broomfield, CO\",\"email\":null,\"hireable\":true,\"bio\":\"Systems-oriented critical thinker with excellentwritten and verbal communication, excited by opportunities to problem-solve and help connect people using tech.\",\"public_repos\":44,\"public_gists\":6,\"followers\":4,\"following\":2,\"created_at\":\"2018-06-29T20:04:02Z\",\"updated_at\":\"2020-01-11T16:19:26Z\"}"
```

To make this unwieldly mash of quotes usable as a Hash without going through the trouble of splitting it along certain separators and cleaning up all the data types, Ruby's built-in JSON module and its #parse class method can be used to quickly reformat it:
```Ruby
hash = JSON.parse(body)
```

Now you have a Ruby-friendly Hash object which you can interact with in all the normal Ruby ways. Slice, dice it, enumerate over it, turn it into another object: the world is your (nested, comma-separated) oyster.

Thanks for reading!

## Resources

- [Intro to APIs - Turing](https://gist.github.com/BrianZanti/e9d73508062fdcb78225906a6d97686d)
- [What Exactly Is an API? - Perry Eising, Medium](https://medium.com/@perrysetgo/what-exactly-is-an-api-69f36968a41f)
- [What Is An API? In English, Please - Petr Gazarov, freeCodeCamp](https://www.freecodecamp.org/news/what-is-an-api-in-english-please-b880a3214a82/)
- [What Is an API? (Video) - MuleSoft](https://www.youtube.com/watch?v=s7wmiS2mSXY)
