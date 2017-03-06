# Arendelle

Arendelle is a small gem with a distantly semantic name, used for initializing mostly frozen objects. If you've got kids, you probably already understand.

## Why?

We primarily use Arendelle in our innternal projects combined with JSON.parse and the `object_class` setting, to initialize (mostly) frozen objects. This helps prevent accidentally mutating the state of a parsed JSON file or configuration class, while providing a javascript object-like interface for accessing deeply nested settings. See usage for more details.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'arendelle'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install arendelle

## Usage

```
# api_keys.yml
defaults: &defaults
  cool_service:
    client_secret: <%= ENV["COOL_SERVICE_CLIENT_SECRET"] || "default_value" %>

# settings.rb
class Settings
  def self.api_keys
    Rails.application.config_for(:api_keys)
  end

  def self.values
    @values ||= JSON.parse(api_keys.to_json, object_class: Arendelle)
  end

  api_keys.each do |(k, _v)|
    define_singleton_method(k) do
      values.send(k)
    end
  end
end
```

```
Settings.cool_service.client_secret
  => "default_value" # assuming the ENV var isn't set
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/arendelle. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

