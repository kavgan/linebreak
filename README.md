Linebreak
=========

[![Gem Version](https://badge.fury.io/rb/linebreak.png)](https://badge.fury.io/rb/linebreak)
[![Build Status](https://secure.travis-ci.org/aef/linebreak.png)](https://secure.travis-ci.org/aef/linebreak)
[![Dependency Status](https://gemnasium.com/aef/linebreak.png)](https://gemnasium.com/aef/linebreak)
[![Code Climate](https://codeclimate.com/github/aef/linebreak.png)](https://codeclimate.com/github/aef/linebreak)
[![Coverage Status](https://coveralls.io/repos/aef/linebreak/badge.png?branch=master)](https://coveralls.io/r/aef/linebreak)

* [Documentation][docs]
* [Project][project]

   [docs]:    http://rdoc.info/github/aef/linebreak/
   [project]: https://github.com/aef/linebreak/

Description
-----------

Linebreak is a Ruby library for conversion of text between linebreak encoding
formats of Unix, Windows or Mac.

A command-line tool is available as separate gem named linebreak-cli.
Earlier versions of Linebreak were called BreakVerter.

Features / Problems
-------------------

This project tries to conform to:

* [Semantic Versioning (2.0.0)][semver]
* [Ruby Packaging Standard (0.5-draft)][rps]
* [Ruby Style Guide][style]
* [Gem Packaging: Best Practices][gem]

   [semver]: http://semver.org/
   [rps]:    http://chneukirchen.github.com/rps/
   [style]:  https://github.com/bbatsov/ruby-style-guide
   [gem]:    http://weblog.rubyonrails.org/2009/9/1/gem-packaging-best-practices

Additional facts:

* Written purely in Ruby.
* Documented with YARD.
* Automatically testable through RSpec.
* Intended to be used with Ruby 1.9.3 or higher.
* Extends core classes. This can be disabled through bare mode.
* Cryptographically signed gem and git tags.

Synopsis
--------

This documentation defines the public interface of the software. Don't rely
on elements marked as private. Those should be hidden in the documentation
by default.

### Loading

In most cases you want to load the library by the following command:

~~~~~ ruby
require 'linebreak'
~~~~~

In a bundler Gemfile you should use the following:

~~~~~ ruby
gem 'linebreak'
~~~~~

By default, Linebreak extends the String class to make all Strings support the
linebreak helper methods. The decision to modify Ruby's core classes should be
in the hand of the end user (e.g. the application developer). So if you don't
want the core extensions to be loaded or if you are using this library as a
component in another library you should load this library in bare mode: 

~~~~~ ruby
require 'linebreak/bare'
~~~~~

Or for bundler Gemfiles:

~~~~~ ruby
gem 'linebreak', require: 'linebreak/bare'
~~~~~

### Library

You can convert Strings to a target linebreak encoding. The default encoding is
:unix. You can also choose :mac and :windows. Notice that the :mac encoding is
deprecated. Modern Apple machines also use :unix linebreak encoding, while
Commodore machines also use the :mac linebreak encoding.

~~~~~ ruby
windows_string = "Abcdef\r\nAbcdef\r\nAbcdef"

windows_string.linebreak_encode(:unix)
#=> "Abcdef\nAbcdef\nAbcdef"
~~~~~

You can detect which decoding systems are used in a String:

~~~~~ ruby
mixed_unix_and_mac_string = "Abcdef\rAbcdef\nAbcdef"

mixed_unix_and_mac_string.linebreak_encodings
#=> #<Set: {:unix, :mac}>
~~~~~

You can also easily ensure that a String contains exactly the linebreak
encodings you expect it to contain:

~~~~~ ruby
mixed_windows_and_mac_string = "Abcdef\r\nAbcdef\rAbcdef"

mixed_windows_and_mac_string.linebreak_encoding?(:windows)
#=> false

mixed_windows_and_mac_string.linebreak_encoding?(:windows, :mac)
#=> true
~~~~~

Note that the expected linebreak systems could also be given as an Array or a
Set.

If you prefer not the use the core extensions (e.g. in bare mode) you can use
this library in the following way: 

~~~~~ ruby
Aef::Linebreak.encode("Abcdef\nAbcdef\nAbcdef", :mac)
#=> "Abcdef\rAbcdef\rAbcdef"

Aef::Linebreak.encodings("Abcdef\r\nAbcdef\nAbcdef")
#=> #<Set: {:unix, :windows}>

Aef::Linebreak.encoding?("Abcdef\nAbcdef\nAbcdef", :unix, :windows)
#=> false

Aef::Linebreak.encoding?("Abcdef\nAbcdef\nAbcdef", :unix)
#=> true
~~~~~

Or you can manually extend a String to support the linebreak helper methods like
this:

~~~~~ ruby
some_string.extend Aef::Linebreak
~~~~~

Requirements
------------

* Ruby 1.9.3 or higher

Installation
------------

On *nix systems you may need to prefix the command with sudo to get root
privileges.

### High security (recommended)

There is a high security installation option available through rubygems. It is
highly recommended over the normal installation, although it may be a bit less
comfortable. To use the installation method, you will need my [gem signing
public key][gemkey], which I use for cryptographic signatures on all my gems.

Add the key to your rubygems' trusted certificates by the following command:

    gem cert --add aef-gem.pem

Now you can install the gem while automatically verifying its signature by the
following command:

    gem install linebreak -P HighSecurity

Please notice that you may need other keys for dependent libraries, so you may
have to install dependencies manually.

   [gemkey]: https://aef.name/crypto/aef-gem.pem

### Normal

    gem install linebreak

### Automated testing

Go into the root directory of the installed gem and run the following command
to fetch all development dependencies:

    bundle

Afterwards start the test runner:

    rake spec

If something goes wrong you should be noticed through failing examples.

Development
-----------

### Bug reports and feature requests

Please use the [issue tracker][issues] on github.com to let me know about errors
or ideas for improvement of this software.

   [issues]: https://github.com/aef/linebreak/issues/

### Source code

#### Distribution

This software is developed in the source code management system Git. There are
several synchronized mirror repositories available:

* GitHub (located in California, USA)
    
    URL: https://github.com/aef/linebreak.git

* Gitorious (located in Norway)
    
    URL: https://git.gitorious.org/linebreak/linebreak.git

* BitBucket (located in Colorado, USA)
    
    URL: https://bitbucket.org/alefi/linebreak.git

* Pikacode (located in France)

    URL: https://pikacode.com/aef/linebreak.git

You can get the latest source code with the following command, while
exchanging the placeholder for one of the mirror URLs:

    git clone MIRROR_URL

#### Tags and cryptographic verification

The final commit before each released gem version will be marked by a tag
named like the version with a prefixed lower-case "v", as required by Semantic
Versioning. Every tag will be signed by my [OpenPGP public key][openpgp] which
enables you to verify your copy of the code cryptographically.

   [openpgp]: https://aef.name/crypto/aef-openpgp.asc

Add the key to your GnuPG keyring by the following command:

    gpg --import aef-openpgp.asc

This command will tell you if your code is of integrity and authentic:

    git tag -v [TAG NAME]

#### Building gems

To package your state of the source code into a gem package use the following
command:

    rake build

The gem will be generated according to the .gemspec file in the project root
directory and will be placed into the pkg/ directory.

### Contribution

Help on making this software better is always very appreciated. If you want
your changes to be included in the official release, please clone the project
on github.com, create a named branch to commit, push your changes into it and
send a pull request afterwards.

Please make sure to write tests for your changes so that no one else will break
them when changing other things. Also notice that an inclusion of your changes
cannot be guaranteed before reviewing them.

The following people were involved in development:

- Alexander E. Fischer

License
-------

Copyright Alexander E. Fischer <aef@godobject.net>, 2009-2013

This file is part of Linebreak.

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH
REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND
FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT,
INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM
LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR
OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
PERFORMANCE OF THIS SOFTWARE.
