# CarthageCache

[![Code Climate](https://codeclimate.com/github/guidomb/carthage_cache/badges/gpa.svg)](https://codeclimate.com/github/guidomb/carthage_cache)
[![Test Coverage](https://codeclimate.com/github/guidomb/carthage_cache/badges/coverage.svg)](https://codeclimate.com/github/guidomb/carthage_cache/coverage)
[![Gem Version](https://badge.fury.io/rb/carthage_cache.svg)](https://badge.fury.io/rb/carthage_cache)

CarthageCache allows Carthage users to have a shared cache of their `Carthage/Build` folder backed by Amazon S3.

Most libraries don't provide pre-compiled binaries, `.framework` files, in their releases. Even if they do, due to Swift lack of ABI, you might be forced to use `--no-use-binaries` flag and compile all your dependencies. Which, depending on the amount of dependencies and their size it could take significant time.

When you add slow building environments like Travis CI to the mix, a project bootstrap could take around 25 minutes just to build all your dependencies. Which is a lot for every push or pull request. You want your build and test to run really fast.

CarthageCache generates a hash key based on the content of your `Cartfile.resolved` and checks if there is a cache archive (a zip file of your `Carthage/Build` directory) associated to that hash. If there is one it will download it and install it in your project avoiding the need to run `carthage bootstrap`.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'carthage_cache'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install carthage_cache

## Setup

### AWS credentials

First of all you need to configure your AWS credentials. You can do this by a `.carthage_cache.yml` file. CarthageCache will try to find this file in the current working directory. It is recommended to generate this file in each project you want to use CarthageCache.

To generate a `.carthage_cache.yml` you just need to run

```
carthage_cache config
```

You can also set your credentials using the following environmental variables

 * `AWS_REGION`
 * `AWS_ACCESS_KEY_ID`
 * `AWS_SECRET_ACCESS_KEY`

### AWS S3 bucket

CarthageCache will assume there is a bucket named `carthage-cache`. You can change the bucket to be used by using the option `-b` or `--bucket-name`.

## Usage

If you want to bootstrap a project from cache and if there is none then fallback to Carthage.

```
carthage_cache install || carthage bootstrap
```

If you want to update dependencies and update cache

```
carthage update && carthage_cache publish
```

If you want to check whether a cache exists for the current `Carfile.resolved`

```
carthage_cache exist
```

For more information run the help command

```
carthage_cache help
```

### Project's root directory

The `carthage_cache` command assumes that the project's root directory is the current working directory and that the `Cartfile.resolved` file is located in the project's root directory. All `carthage_cache` accept an optional argument to set the project's root directory. For example

```
carthage_cache install
```
Will try to read the `Cartfile.resolved` file from the current working directory and will install the cache archive in `./Carthage/Build`.

```
carthage_cache install PATH/TO/MY/PROJECT
```
Will try to read the `PATH/TO/MY/PROJECT/Cartfile.resolved` and will install the cache archive in `PATH/TO/MY/PROJECT/Carthage/Build`.



## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/guidomb/carthage_cache/issues/new.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
