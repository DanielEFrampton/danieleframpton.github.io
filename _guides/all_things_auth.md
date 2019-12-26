---
layout: default
title: 'All Things Auth'
---

# All Things Auth: Authentication & Authorization in Rails

By [Daniel Frampton](https://gist.github.com/DanielEFrampton), student in the Back-End Engineering program at [Turing School of Software & Design](http://turing.io).

## Definitions

"Whoa hey wait. Aren't those the same things?"

Yeah nope. I'll give some examples from your own experience as a web user, but briefly this is what they are:

- Authentication: making sure people are who they claim to be.

- Authorization: giving different people access to different actions depending on user type.

## Authentication

We're all familiar with authentication from the user perspective. We login to websites every day using some kind of username and a password. What is less apparent is how that process works under the hood.

### Registration & Passwords

In order to not create a calamity of epic proportions, or what one of my instructors calls a "CEM" (career-ending move), you never store passwords in your database in an unencrypted state lest an aspiring hacker were to gain access to them. (Usernames are not as great a concern because they are frequently publicly accessible anyway.) But if it's best practice to encrypt passwords, you might ask, how do you check them against what the user enters when logging in?

The answer is through the use of _hashing functions_. Hashing functions run a given piece of information--usually some kind of string (i.e., text and numbers and so forth, like a password)--through what is called a "one-way" algorithm to produce a long, complex string of characters called a hash. It is "one-way" because because it is very difficult to undo the process and go from the hash back to the password. If it is a good hashing function, only the exact original piece of information can produce that particular hash. Therefore, the hash can stand in for the password.

When a user like yourself logs into a website, the same hashing algorithm that encrypted the password in the first place is used again to hash it and then check it against the hashed password in the database corresponding to your username. In this way, the unencrypted password is stored in memory only long enough to hash it and it's never saved anywhere in an unencrypted state, keeping your user's password secure and yourself free of risky liability.

In Rails, this is all accomplished through an encyrption gem called Bcrypt and Rail's built-in SecurePassword library which is intended to work alongside Bcrypt. The steps to set it up are roughly as follows:

1. Require `bcrypt` in your project's Gemfile and run `bundle install` and `bundle update`.
1. In the model where you store user information (e.g., `User`) and in your registration and login forms, create text fields and validation rules for the users to input a `password` and optionally a `password_confirmation`.
1. In the related table, create a `password_digest` column of the `string` data type.
1. On the model, add the `use_secure_password` keyword. This tells Rails to expect a column beginning with a word of your choice ending with `_digest`, like `password_digest`, and to encrypt something input to a field with that word (i.e., `password`) and optionally one ending in `_confirmation` used for checking against typos (i.e., `password_confirmation`) and store its hashed value in `password_digest`.

One other use of Bcrypt is necessary and it occurs during login of a previously registered user, which is described more fully below. Bcrypt and Rails provide an `.authenticate(password)` method to objects with the `has_secure_password` keyword which takes a password, hashes it, and checks it against the hashed password stored in whatever `_digest` column exists in the database for that model, returning true or false.

### Login and Sessions

How many people can be logged in to Facebook at a given time? Amazon? How do their servers know whether you're logged in or not at your particular computer in your particular browser? They do it by storing temporary but secure data about a particular user in a _session_, a server-controlled file that works very similarly to a cookie except it is encrypted and unalterable by users. Most web frameworks store their sessions on the server itself, but by default Rails stores its sessions on the respective clients' computers to save storage space and improve speed.

When a user logs into an app, a session is created

(creating a shared current_user method to access the user across all views)

(saving the user_id to session when logging in)

(deleting the user_id key/value pair from session when logging out)


## Authorization

Unlike authentication, authorization is something we're not likely to be as familiar with as web users; or at least, we weren't aware of it. If you've ever created a Facebook group or event, created an event on Evite, or opened a new channel on Slack, you've experienced having a different level of access than other users in a web app: perhaps the ability to invite new people, change the details about a group or event, or to delete that group or event. Authorization is the process of granting such different levels of access to different groups of users.

(namespacing for organization of files)

(enums to track different roles and gain methods to check them on current_user)

(inherited before_action methods on namespaced controllers to restrict access to particular roles)

(using partials to maintain shared content across views in different namespaces)

## Additional Reading

- [Sessions, Cookies, and Authentication - The Odin Project](https://www.theodinproject.com/courses/ruby-on-rails/lessons/sessions-cookies-and-authentication)
- [Simple Authentication Guide with Ruby on Rails](https://levelup.gitconnected.com/simple-authentication-guide-with-ruby-on-rails-16a6255f0be8)
- [ActiveRecord::Enum - Rails Docs](https://edgeapi.rubyonrails.org/classes/ActiveRecord/Enum.html)
- [Chapter 8: Log in, log out - Ruby on Rails Tutorial, Michael Hartl](https://3rd-edition.railstutorial.org/book/log_in_log_out)
- [Authentication - Turing School Backend Curriculum](https://backend.turing.io/module2/lessons/authentication)
- [Authorization - Turing School Backend Curriculum](https://backend.turing.io/module2/lessons/authorization)
