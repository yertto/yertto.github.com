---
layout:       default
body_id:      home
title:        yertto
---

Projects
---

* [miff](miff)


What's with all these command line tools?
---

I get [ruby](http://ruby-doc.org) and [irb](http://ruby-doc.org/docs/ProgrammingRuby/html/irb.html),
but what's with all these other command line tools?

* [gem](http://docs.rubygems.org/read/chapter/9)
* [rake](http://rake.rubyforge.org)
* [thor](http://github.com/wycats/thor)
* [bundler](http://gembundler.com)
* [jeweler](http://technicalpickles.com/posts/craft-the-perfect-gem-with-jeweler/)
* [rvm](http://rvm.beginrescueend.com)

Do we really need all of them?
Can't they be combined or merged or something?
I'm so looking forward to the day this all gets cleaned up.


Installing gems
---
Currently done with:

  $ gem install

And now, a warm welcome to [bundler](http://gembundler.com).
(An awesome tool for managing an applications dependencies, including install.)

But why do we need two tools to install gems?
I'm hoping that "bundler" will get merged into the "gem" tool.
(*especially* since gem doesn't manage dependencies.)

Also, i think a few things need to be combined and renamed.

eg.

  Gemfile      -> foo.gem_reqs
  Gemfile.lock -> foo.gem_spec  # ie. a dependency tree with a gemspec
  foo.gemspec  -> foo.gem_spec  # ie. a gemspec with a dependency tree

then, to fetch and install a gem from a Gemserver (as normal),

  $ gem install foo   # fetches foo.gem and installs it

And to install current app and its dependencies,
(ie. what "bundle install" does)

  $ cd foo
  $ gem install       # looks at dependencies in foo.gem_spec

where,

  foo.gem_spec

  is created by the bastard child of [jeweler](http://technicalpickles.com/posts/craft-the-perfect-gem-with-jeweler/) and [bundler](http://gembundler.com)
  so that it will add the dependency tree to the foo.gem_spec file.


Creating gems
---
So, the gem developer just has to worry about editing,
  
  foo.gem_reqs    # for gem requirements  (formally Gemfile)

and creating,

  foo.gem_spec

using a *new* (dependency aware) jeweler supplied [rake](http://rake.rubyforge.org) task:

  $ rake gem_spec            # ie. "rake gemspec" + "bundle lock"

while still using the existing [jeweler](http://technicalpickles.com/posts/craft-the-perfect-gem-with-jeweler/) supplied [rake](http://rake.rubyforge.org) tasks:

  $ rake version:bump:minor  # bump version number
  $ rake build               # build a gem
  $ rake release             # release a gem to the world


when that happens it *should* be possible to install gems straight
from the command line, fetched from git repos, using the "gem" tool.

eg.

  $ gem install --tag=v0.0.2 git://github.com/foo/foo.git


[jeweler](http://technicalpickles.com/posts/craft-the-perfect-gem-with-jeweler/)
---
[jeweler](http://technicalpickles.com/posts/craft-the-perfect-gem-with-jeweler/) tool is another command line tool we don't need.

they should also merge the "jeweler" tool with the "gem" tool.

"jeweler [options] foo" --> "gem create [options] foo"

eg.

  $ gem create --create-repo --summary "Pretty fooey" foo

although jeweler's options,

  --create-repo

should be changed to,

  --create-github

for the day when github is no longer *the* place to store code.


[rake](http://rake.rubyforge.org)
---
Oh yer [rake](http://rake.rubyforge.org), do we need that?
Can we use [thor](http://github.com/wycats/thor) to merge it in with
[gem](http://docs.rubygems.org/read/chapter/9)?


Summary
---
 { gem create foo } =  { jeweler foo    }
 { rake           } += { thor           }
 { rake gem_spec  } += { bundle lock    }
 { gem install    } += { bundle install }
