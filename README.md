# Rails5 Spec Converter

[![Build Status](https://travis-ci.org/tjgrathwell/rails5-spec-converter.svg?branch=master)](https://travis-ci.org/tjgrathwell/rails5-spec-converter)

A script that fixes the syntax of your tests so Rails 5 will like them better. Inspired by [transpec](https://github.com/yujinakayama/transpec), the RSpec 2 -> 3 syntax conversion tool.

If you write a test like this:

```
get :users, search: 'bayleef', format: :json
expect(response).to be_success
```

Rails 5 will issue a hearty deprecation warning, persuading you to write this instead:

```
get :users, params: { search: 'bayleef' }, format: :json
expect(response).to be_success
```

This is great! That syntax is great. However, if you have a thousand tests lying around, it will probably be very time consuming to find all the places where you need to fix that.

## Installation

Install the gem standalone like so:

    $ gem install rails5-spec-converter

## Usage

Make sure you've committed everything to Git first, then

    $ cd some-project
    $ rails5-spec-converter

This will update all the files in that directory matching the globs `spec/**/*_spec.rb` or `test/**/*_test.rb`.

If you want to specify a specific set of files instead, you can run `rails5-spec-converter path_to_my_files`.

By default it will make some noise, run with `rails5-spec-converter --quiet` if you want it not to.

### Whitespace

#### Indentation

The tool will attempt to indent the newly-added "params" hash in situations when the arguments are on newlines, e.g.:

```
  get :index
      search: 'bayleef',
      format: :json
```

becomes

```
  get :index
      params: {
        search: 'bayleef'
      },
      format: :json
```

Since the extra spaces in front of 'params' are brand-new whitespace, you may want to configure them (default is 2 spaces).

`rails5-spec-converter --indent '    '`

`rails5-spec-converter --indent '\t'`

#### Hash Spacing

By default, for single-line hashes, a single space will be added after the opening curly brace and before the ending curly brace. The space will be omitted if the new params hash will contain any hash literals that do not have surrounding whitespace, ex:

```
post :users, user: {name: 'bayleef'}
```

becomes

```
post :users, params: {user: {name: 'bayleef'}}
```

* `--no-hash-spacing` will force hashes to be written **without** extra whitespace in all files regardless of context.

* `--hash-spacing` will force hashes to be written **with** extra whitespace in all files regardless of context.

## Compatibility

It **should** work for both RSpec and MiniTest, but who really knows?

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `bundle exec rspec spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/tjgrathwell/rails5-spec-converter. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](contributor-covenant.org) code of conduct.

## Contact

If this Gem helped you out at all, or it didn't help because you wanted it to do something different or it broke all your computer code, please let me know on twitter [@tjgrathwell](http://twitter.com/tjgrathwell)

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

