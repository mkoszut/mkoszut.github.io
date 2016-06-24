---
title:  "A Draft Post"
description: Work in progress
## date: add a date when publishing
---

Drafts are posts without a date. They’re posts you’re still working on and don’t want to publish yet. To get up and running with drafts, check the _drafts folder in the site’s root.

To preview your site with drafts, simply run **`jekyll serve`** or **`jekyll build`** with the **`--drafts`** switch. Each will be assigned the value modification time of the draft file for its date, and thus you will see currently edited drafts as the latest posts.

You'll find this post in your `_posts` directory - edit this post and re-build (or run with the `-w` switch) to see your changes!
To add new posts, simply add a file in the `_posts` directory that follows the convention: YYYY-MM-DD-name-of-post.ext.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
