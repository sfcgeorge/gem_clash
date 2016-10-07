# Potential Bundler Secondary Source Security Issue

See my [other repo](https://github.com/sfcgeorge/eve) for a sample app showing this flaw.

Even if a secondary source is limited to a single gem, Bundler will mistakenly look at all sources for every non limited gem. This could pose a security issue depending on how you use secondary sources and where trust is.

In practice I can't think of real world scenarios where this would cause any more risk than say a gem author inserting malicious code into their own gem, but it's something to think about.

I was alerted me to this by [Steve's blog post](http://collectiveidea.com/blog/archives/2016/10/06/bundlers-multiple-source-security-vulnerability/).

## Gemfile

```ruby
source "https://rubygems.org"

gem "rack"
gem "eve", git: "https://github.com/sfcgeorge/eve.git"
```

## bundle

```sh
Fetching https://github.com/sfcgeorge/eve.git
Fetching gem metadata from https://rubygems.org/..........
Fetching version metadata from https://rubygems.org/.
Resolving dependencies...
Using eve 1.0.0 from https://github.com/sfcgeorge/eve.git (at master@303e489)
Using rack 9.9.9 from https://github.com/sfcgeorge/eve.git (at master@303e489)
Using bundler 1.13.2
Bundle complete! 2 Gemfile dependencies, 3 gems now installed.
Use `bundle show [gemname]` to see where a bundled gem is installed.
Post-install message from rack:
😈 EVIL fake Rack just installed! 👿
```

Note how "rack" is installed from my malicious repo even though we'd expect it to come from RubyGems!
