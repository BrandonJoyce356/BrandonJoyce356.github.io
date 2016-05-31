---
layout: post
title: Saving a Base64 Encoded File w/ Elixir
permalink: saving-base64-encoded-file-elixir
---

Just wanted to document this for future use:

{% highlight elixir %}
{:ok, data} = Base.decode64(base64_string)
File.write("/tmp/file.gif", data, [:binary])
{% endhighlight %}

That's it!
