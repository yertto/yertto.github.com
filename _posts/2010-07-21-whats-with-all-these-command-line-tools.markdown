---
layout:       post
title:        What's with all these command line tools?
---

# {{ page.title }}

I get [ruby](http://ruby-doc.org) and [irb](http://ruby-doc.org/docs/ProgrammingRuby/html/irb.html),
but what's with all these other command line tools?

* [gem][1]
* [rake][2]
* [thor][3]
* [bundler][4]
* [jeweler][5]

Do we really need all of them?
Can't they be combined or merged or something?
I'm so looking forward to the day this all gets cleaned up.


## Installing gems

Currently done with:

{% highlight sh %}
gem install
{% endhighlight %}


And now, a warm welcome to [bundler][4].
(An awesome tool for managing an applications dependencies, including install.)

But why do we need two tools to install gems?
I'm hoping that [bundler][4] will get merged into the [gem](http://docs.rubygems.org/read/chapter/9) tool.
(*especially* since [gem](http://docs.rubygems.org/read/chapter/9) doesn't manage dependencies.)

Also, i think a few things need to be combined and renamed.

eg.

{% highlight ruby %}
  Gemfile      -> foo.gem_reqs
  Gemfile.lock -> foo.gem_spec  # ie. a dependency tree with a gemspec
  foo.gemspec  -> foo.gem_spec  # ie. a gemspec with a dependency tree
{% endhighlight %}

then, to fetch and install a gem from a Gemserver (as normal),

{% highlight sh %}
  $ gem install foo   # fetches foo.gem and installs it
{% endhighlight %}

And to install current app and its dependencies,
(ie. what "bundle install" does)

{% highlight sh %}
  $ cd foo
  $ gem install       # looks at dependencies in foo.gem_spec
{% endhighlight %}

where,

{% highlight ruby %}
  foo.gem_spec
{% endhighlight %}

is created by the bastard child of [jeweler][5] and [bundler][4]
so that it will add the dependency tree to the foo.gem_spec file.


## Creating gems

So, the gem developer just has to worry about editing,
  
{% highlight ruby %}
  foo.gem_reqs    # for gem requirements  (formally Gemfile)
{% endhighlight %}

and creating,

{% highlight ruby %}
  foo.gem_spec
{% endhighlight %}

using a *new* (dependency aware) [jeweler][5] supplied [rake][2] task:

{% highlight sh %}
  $ rake gem_spec            # ie. "rake gemspec" + "bundle lock"
{% endhighlight %}

while still using the existing [jeweler][5] supplied [rake][2] tasks:

{% highlight sh %}
  $ rake version:bump:minor  # bump version number
  $ rake build               # build a gem
  $ rake release             # release a gem to the world
{% endhighlight %}


when that happens it *should* be possible to install gems straight
from the command line, fetched from git repos, using the "gem" tool.

eg.

{% highlight sh %}
  $ gem install --tag=v0.0.2 git://github.com/foo/foo.git
{% endhighlight %}


## [jeweler][5]

[jeweler][5] tool is another command line tool we don't need.

they should also merge the [jeweler][5] tool with the "gem" tool, so

{% highlight sh %}
jeweler [jeweler-options] foo
{% endhighlight %}

becomes,

{% highlight sh %}
gem create [jeweler-options] foo
{% endhighlight %}


eg.

{% highlight sh %}
  $ gem create --create-repo --summary "Pretty fooey" foo
{% endhighlight %}

although jeweler's options,

{% highlight sh %}
  --create-repo
{% endhighlight %}

should be changed to,

{% highlight sh %}
  --create-github
{% endhighlight %}

for the day when github is no longer *the* place to store code.


## [rake][2]

Oh yer [rake][2], do we need that?
Can we use [thor][3] to merge it in with
[gem](http://docs.rubygems.org/read/chapter/9)?


## Summary

{% highlight ruby %}
 { gem create foo } =  { jeweler foo    }
 { rake           } += { thor           }
 { rake gem_spec  } += { bundle lock    }
 { gem install    } += { bundle install }
{% endhighlight %}


[1]: http://docs.rubygems.org/read/chapter/9                                "gem"
[2]: http://rake.rubyforge.org                                              "rake"
[3]: http://github.com/wycats/thor                                          "thor"
[4]: http://gembundler.com                                                  "bundler"
[5]: http://technicalpickles.com/posts/craft-the-perfect-gem-with-jeweler/  "jeweler"
