# RailsQuery

Layer above ActiveRecord to define your models query interface.

### Features

* Define fields
* Define filters
* Define links
* Define methods
* Get database response directly without model instances

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rails_query'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install rails_query

## Example

```ruby
class UserQuery < RailsQuery::Query
  model User

  field :name, default: true
  field :lastname, filter: :contain
  field :age

  field :country_name, join: {region: :country}

  field :fullname, select: "name || ' ' || lastname"

  field :region_id

  link_one :region, query: RegionQuery, key: :region_id

  method :now, ->(_row) { Time.new(2020).utc }

  filter :under_age, ->(_val) { where(age: 1..17) }

  filter :age_gt, type: :gt, field: :age
  filter :age_lt, type: :lt, field: :age
  filter :age_range, type: :range, field: :age
end

UserQuery.new.select(:country_name)
             .include(:region)
             .filtrate(under_age: true)
             .page(2)
             .limit(1)
             .order(name: 'desc')
             .run
```

## Usage

TODO: Write usage instructions here

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/rails_query. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/[USERNAME]/rails_query/blob/master/CODE_OF_CONDUCT.md).


## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the RailsQuery project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/[USERNAME]/rails_query/blob/master/CODE_OF_CONDUCT.md).
