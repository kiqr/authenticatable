Authenticatable 
==============
[![RuboCop Github Action](https://github.com/kiqr/authenticatable/actions/workflows/rubocop.yml/badge.svg)](https://github.com/kiqr/authenticatable/actions/workflows/rubocop.yml)
[![RSpec](https://github.com/kiqr/authenticatable/actions/workflows/rspec.yml/badge.svg)](https://github.com/kiqr/authenticatable/actions/workflows/rspec.yml)
[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE.md)

An authentication framework based on [Warden](https://github.com/wardencommunity/warden) that provides a set of **security features**, strategies and helpers to build your own customized authentication logic.

Installation
------------

Add the following line to Gemfile:

```ruby
gem "authenticatable", "~> 2.0"
```

and run `bundle install` from your terminal to install it.


Examples
--------
All examples below expects that you've already configured an authenticatable model with class name `User` and scope `:user`.

#### Signing in with email & password
```ruby
user = User.find_by(email: params[:email])

if user&.authenticate(params[:password])
  sign_in!(user)
  redirect_to user_path(user), notice: "You've signed in!"
else
  flash.now[:alert] = "Wrong username or credentials"
  render :new
end
```

#### Signing in with OmniAuth
```ruby
class SessionsController < ApplicationController
  skip_before_action :verify_authenticity_token, only: :callback

  def callback
    auth_hash = request.env['omniauth.auth']
    @user = User.find_or_create_from_auth_hash(auth_hash)
    sign_in! @user
    redirect_to user_path(@user)
  end
end
```

#### Retrieving the current authenticated user.
```ruby
current_user => <User>|nil
```

#### Check if a user is authenticated.
```ruby
user_signed_in? => true|false
```

Contributing
------------
If you are interested in reporting/fixing issues and contributing directly to the code base, please see [CONTRIBUTING.md](CONTRIBUTING.md) for more information on what we're looking for and how to get started.

Versioning
----------
This library aims to adhere to [Semantic Versioning](http://semver.org/). Violations
of this scheme should be reported as bugs. Specifically, if a minor or patch
version is released that breaks backward compatibility, that version should be
immediately yanked and/or a new version should be immediately released that
restores compatibility. Breaking changes to the public API will only be
introduced with new major versions. As a result of this policy, you can (and
should) specify a dependency on this gem using the [Pessimistic Version
Constraint](http://guides.rubygems.org/patterns/#pessimistic-version-constraint) with two digits of precision. For example:

```ruby
gem "authenticatable", "~> 2.0"
```

License
-------
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
