# ActiveModelStruct

Serializer for ActiveModel based on Ruby OpenStruct standard library.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'active_model_struct'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install active_model_struct

## Usage

### Grape

```ruby
class API < Grape::API
  formatter :json, ActiveModelStruct::Formatter
  
  namespace :users do
    desc 'Get Users'
    get :users, each_serializer: UserSerializer do
      User.all
    end
    
    desc 'Get User by ID'
    get 'users/:id', serializer: UserSerializer do
      User.find(params[:id])
    end
    
    desc 'Update User'
    params do
      requires :username, type: String
    end
    patch 'users/:id' do
      User.find(params[:id]).update(declared(params))
    end
    
    desc 'Create User'
    params do
      requires :username, type: String
      requires :password, type: String
    end
    post :users do
      user = User.create(declared(params))
      
      # you can pass which fields should be serialized
      # with UserSerializer in param :fields 
      render user, fields: [:id, :username]
    end
  end
end
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/rufoos/active_model_struct.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
