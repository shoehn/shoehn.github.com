---
layout: post
title: Testing the code highlighter
tags:
- code
---
I just wanted to have a look how the highlight tag renders different sources.
For ruby:
{% highlight ruby linenos %}
def foo
  puts 'foo'
end
{% endhighlight %}

Haskell:
{% highlight hs linenos %}
module Main () where
import Data.Text
main :: IO ()
main = putStrLn "Hallo Welt!"
{% endhighlight %}

and of course Erlang:

{% highlight erlang linenos %}
to_html(ReqData, State) ->
  {"<html><body>", ReqData, State}.
{% endhighlight %}