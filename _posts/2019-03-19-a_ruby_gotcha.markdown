---
layout: post
title:      "A ruby gotcha"
date:       2019-03-19 22:01:45 -0400
permalink:  a_ruby_gotcha
---


I see this issue with the `.each` method come up a lot so I thought we could take a closer look.

```
words = ["apple", "pear", "orange"]

def to_caps(words) 
  upcased_words = []
  words.each do |word|
    upcased_words << word.upcase
  end
end
```

What does this method return? If you said, `["apple", "pear", "orange"]`, you're right! But, why is this happening? 

Since we are not explicitly using the `return` keyword here, our method is implicitly returning the last expression evaluated. The last expression evaluated turns out to be `words.each {...}`.

`.each` will evaluate to the array it is called on so `words.each {...}` will evaluate to `words` and since that is the last expression evaluated, our method implicitly returns `words`. 

To return our `upcased_words` we need to make that variable the last expression evaluated so our method implicitly returns it:

```
words = ["apple", "pear", "orange"]

def to_caps(words) 
  upcased_words = []
  words.each do |word|
    upcased_words << word.upcase
  end
  upcased_words
end
```

