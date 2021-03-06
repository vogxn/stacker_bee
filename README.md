# Unofficial Ruby CloudStack client

[![Code Climate](https://codeclimate.com/github/promptworks/stacker_bee.png)](https://codeclimate.com/github/promptworks/stacker_bee)
[![Dependency Status](https://gemnasium.com/promptworks/stacker_bee.png)](https://gemnasium.com/promptworks/stacker_bee)
[![Build Status](https://travis-ci.org/promptworks/stacker_bee.png?branch=master)](https://travis-ci.org/promptworks/stacker_bee)
[![Gem Version](https://badge.fury.io/rb/stacker_bee.png)](http://badge.fury.io/rb/stacker_bee)

The unofficial CloudStack client for Ruby.

## Installation

You can install StackerBee with rubygems:

    $ gem install stacker_bee

If you are using Bundler simply add the following to your Gemfile:

    gem 'stacker_bee'

And execute:

    $ bundle install


## Basic Usage

    cloud_stack = StackerBee::Client.new(
      url:        'http://localhost:8080/client/api',
      api_key:    'MY_API_KEY',
      secret_key: 'MY_SECRET_KEY'
    )

    cloud_stack.list_virtual_machines state: 'Running'
    # => [ { id: '48b91ab4...', displayName: '...', ... },
    #      { id: '59c02bc5...', displayName: '...', ... },
    #      ... ]

    cloud_stack.create_volume name: 'MyVolume'

## Features

### Idomatic Ruby formatting for names

You can use `list_virtual_machines` instead of `listVirtualMachines` and
`affinity_group_id` instead of `affinitygroupid` (if you want to).

For example:

    vm = cloud_stack.list_virtual_machines(affinity_group_id: id).first
    puts vm[:iso_display_text]

### Configurable Logger

    StackerBee::Client.logger = Rails.logger

Or

    StackerBee::Client.logger = Logger.new

### Configurable API Version

By default, StackerBee uses the CloudStack 4.2 API, but it doesn't have to.
Use a different API version by setting the `api_path` configuration option to the path of a JSON file containing the response from your CloudStack instance's `listApis` command.

    StackerBee::Client.api_path = '/path/to/your/listApis/response.json'

### CloudStack REPL

Usage:

    $ stacker_bee [OPTIONS]

Example:

    $ stacker_bee -u http://localhost:8080/client/api -a MY_API_KEY -s MY_SECRET_KEY
    StackerBee CloudStack REPL
    >> list_virtual_machines state: 'Running'
    => [{"id"=>"48b91ab4-dc23-4e24-bc6f-695d58c91087",
      "name"=>"MyVM",
      "displayname"=>"My VM",
      ...
    >>

## Configuration

Configuring a client:

    cloud_stack = StackerBee::Client.new(
      url:        'http://localhost:8080/client/api'
      api_key:    'API_KEY',
      secret_key: 'SECRET_KEY',
      logger:     Rails.logger
    )

All configuration parameters set on the `StackerBee::Client` class are used as defaults for `StackerBee::Client` instances.

    StackerBee::Client.url    = 'http://localhost:8080/client/api'
    StackerBee::Client.logger = Rails.logger

    user_client = StackerBee::Client.new(
      api_key:    'USER_API_KEY',
      secret_key: 'USER_SECRET_KEY'
    )

    root_client = StackerBee::Client.new(
      api_key:    'ROOT_API_KEY',
      secret_key: 'ROOT_SECRET_KEY'
    )

### URL

The URL of your CloudStack instance's URL.

    StackerBee::Client.url = 'http://localhost:8080/client/api'

Or:

    my_client = StackerBee::Client.new(
      url: 'http://localhost:8080/client/api'
    )

### Keys

Your CloudStack credentials, i.e. API key and secret key.

    StackerBee::Client.api_key    = 'MY_API_KEY'
    StackerBee::Client.secret_key = 'MY_SECRET_KEY'

Or:

    my_client = StackerBee::Client.new(
      api_key:    'MY_API_KEY',
      secret_key: 'MY_SECRET_KEY'
    )

### Logger

StackerBee can be configured with a custom `Logger`.

    StackerBee::Client.logger = Logger.new

Or

    my_client = StackerBee::Client.new(
      logger: Logger.new
    )

For Rails apps, it's sometimes convenient to use Rail's logger:

    StackerBee::Client.logger = Rails.logger

### Bulk Configuration

The StackerBee::Client class can be configured with multiple options at once.

    StackerBee::Client.default_config = {
      url:        'http://localhost:8080/client/api',
      logger:     Rails.logger,
      api_key:    'API_KEY',
      secret_key: 'MY_SECRET_KEY'
    }

## Contributing

### Testing

Running the tests:

    $ bundle exec rake

### Testing against CloudStack

To interact with a real CloudStack server, instead of the recorded responses:

    $ cp config.default.yml config.yml

And edit `config.yml`, specifying the URL and credentials for your CloudStack server. This file is used by the test suite if it exists, but is ignored by git.

### Coding Style

This project uses [Rubocop](https://github.com/bbatsov/rubocop) to enforce code style.

    $ bundle exec rubocop

## Thanks to

- [Chip Childers](http://github.com/chipchilders) for a [reference implementation of a CloudStack client in Ruby](http://chipchilders.github.io/cloudstack_ruby_client/)

## License

StackerBee is released under the [MIT License](http://www.opensource.org/licenses/MIT).
