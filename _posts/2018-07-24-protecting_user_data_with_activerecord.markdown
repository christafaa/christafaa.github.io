---
layout: post
title:      "Protecting user data with ActiveRecord"
date:       2018-07-24 23:33:57 -0400
permalink:  protecting_user_data_with_activerecord
---

ActiveRecord and bcrypt work together to protect user information by securing passwords. ActiveRecord’s ActiveModel::SecurePassword module gives us access to the `has_secure_password` class method - all you have to do is call the method in your model:

```
class User < ActiveRecord::Base
   has_secure_password
end
```

Calling `has_secure_password` will create the following methods that we can use on our objects:

* **password=(unencrypted_password)** accepts a password, encrypts it, and saves it to the `password_digest`attribute.
* **authenticate(unencrypted_password)** accepts a password and returns the object it was called on if the password is correct, otherwise it returns false.
* **password_confirmation=(unencrypted_password)** accepts a password and saves it as an instance variable.

`password=(unencrypted_password)` is particularly interesting because it uses bcrypt to generate an encrypted password by taking the argument, turning it into a unique hash, adding a salt (a separate, random string) to the hash, and saving the result to the database.

“Adding a salt means that an attacker has to have a gigantic database for each unique salt – for a salt made of 4 letters, that’s 456,976 different databases. Pretty much no one has that much storage space, so attackers try a different, slower method – throw a list of potential passwords at each individual password. This is much slower than the big database approach, … bcrypt(), though, is designed to be computationally expensive” -[bcrypt-ruby](https://github.com/codahale/bcrypt-ruby)

When a user attempts to login, we can use `authenticate(unencrypted_password)` on our user object to confirm that the submitted password matches the password that was jumbled and saved to the user’s `password_digest` attribute.

The `has_secure_password` macro is a small piece of code that does a lot of work to ensure that passwords can not be maliciously accessed and that we can still authenticate users when they attempt to login.
The content of your blog post goes here.
