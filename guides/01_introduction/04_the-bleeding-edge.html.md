---
chapter: Introduction
title: The Bleeding Edge
---

# The Bleeding Edge

You have three ways of using Padrino edge; the first one is using the git source
code in a gem file, the second one is using a vendored version, and the third is
to install edge into system gems from repository.

**Git in Gemfile** is suitable for those guys that **only** want Padrino's
  latest bleeding edge code.

**System Gems** is suitable for people that want to use the latest padrino and
  freely use `padrino g` and `padrino`.

**Path in Gemfile** is recommended for developers because they can share their
  changes and merge repos between projects.

## Git in Gemfile

```ruby
# Edit Gemfile
gem 'padrino', :git => "git://github.com/padrino/padrino-framework.git"
```

and from console:

```shell
$ bundle install
```

after that you need to run your app in the bundler environment because if you
call directly:

```shell
$ padrino g admin
```

you will use system wide gems. So for do that remember to run commands with
`bundle exec` as a prefix like:

```shell
$ bundle exec padrino start
$ bundle exec padrino g controller foo
$ bundle exec padrino g admin
$ bundle exec padrino g model post
```

You can find more info about bundler usage on their [site](http://bundler.io/
"Link bundler site")

--------------------------------------------------------------------------------

## System Gems from Repository

If you want to install the padrino edge gems into your system rubygems, simply
follow the following steps. First, clone the padrino repository:

```shell
$ cd /tmp
$ git clone git://github.com/padrino/padrino-framework.git
```

Next, we should mark the version as dev(elopment) to get a fresh set of gems:

```ruby
# /tmp/padrino-framework/padrino-core/lib/padrino-core/version.rb
module Padrino
  VERSION = '0.13.1.beta1' unless defined?(Padrino::VERSION) # Change to bump version
  #...
end
```

Finally, run the `fresh` rake command to install the latest version:

```shell
padrino-framework$ sudo rake fresh
```

this will install the latest 'edge' gems into rubygems. Be sure to verify your
project's Gemfile depends on the edge version you installed:

```ruby
Gemfile
# Padrino
gem 'padrino', 'X.X.X'  # This should be the version you installed earlier
```

or you can generate a new project easily and you can use padrino commands
normally:

```shell
padrino g project test-project
```

This should allow you to use the latest padrino code from your system.

--------------------------------------------------------------------------------

## Path in Gemfile

```shell
$ mkdir /src
# Remember to use your fork
$ git clone git://github.com/padrino/padrino-framework.git
# Edit your ~/.profile or ~/.bash_profile or some and add
alias padrino-dev="/src/padrino-framework/padrino-core/bin/padrino"
alias padrino-dev-gen="/src/padrino-framework/padrino-gen/bin/padrino-gen" # you can omit this
```

Reload source or open a new terminal window and check if you have _padrino-dev_
and _padrino-dev-gen_ commands correctly added to your path.

Create a new padrino project:

```shell
padrino-dev-gen project test-project --dev
```

or if you don't have `padrino-dev-gen`

```shell
padrino-dev g project test-project --dev
```

This will append the following lines to your test-app/Gemfile:

```ruby
# Vendored Padrino
%w(core gen helpers mailer admin).each do |gem|
  gem 'padrino-' + gem, :path => "/src/padrino-framework/padrino-" + gem
end
```

Make changes accordingly to your _/src/padrino-framework_ to see them reflected
through your padrino project now using your own vendored version of the
framework.

**REMEMBER** to add always `--dev` when generating a project because without
  that the generated project will use the gem instead the git checkout.
