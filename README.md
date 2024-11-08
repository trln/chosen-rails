# Chosen for rails asset pipeline (TRLN fork)

[Chosen](https://github.com/harvesthq/chosen) is a library for making long, unwieldy select boxes more user friendly.

The `trln-chosen-rails` gem is a fork of the original `chosen-rails`
gem and integrates the `Chosen` with the Rails asset pipeline. This fork removes dependencies on `coffee-rails` and on any particular Sass implementation (the original had a dependency on `sassc-rails`).

One tradeoff is that the version of the `chosen` library is frozen at version 1.8.7.

To make full use of this gem you will need to either add a Sass compilation
step to your asset handling or compile the bundled
`vendor/assets/stylesheets/chosen-base.scss` file you can find in this gem's
directory and add that to your Rails application's `assets/stylesheets` directory.  Since the TRLN Blacklight implementations all employ the use of Sass, this should be unnecessary for members of the consortium.

## Usage

### Install chosen-rails gem

Include `trln-chosen-rails` in Gemfile

```
gem 'trln-chosen-rails'
```

Then run `bundle install`

### About JQuery

You can get jquery via [jquery-rails](https://github.com/rails/jquery-rails).

### Include chosen javascript assets

Add to your `app/assets/javascripts/application.js` if use with jQuery

```coffee
//= require jquery
//= require chosen-jquery
```

### Include chosen stylesheet assets

Add to your `app/assets/stylesheets/application.css`

```scss
*= require chosen
```

### Enable chosen javascript by specific css class

For rails 6, remember to add `javascript_include_tag` in `app/views/layouts/application.html.erb`, like

```ruby
<%= javascript_include_tag 'application' %>
```

Add the class to your form field

```erb
<%= f.select :author,
  User.all.map { |u| [u.name, u.id] },
               { include_blank: true },
               { class: 'chosen-select' }
%>
```

If you use simple form as form builder

```erb
<%= f.association :author,
                  collection: User.all,
                  include_blank: true,
                  input_html: { class: 'chosen-select' }
%>
```

### Deployment

Non-digested assets of `trln-chosen-rails` will simply be copied from digested assets.

## RSpec helpers
`chosen-rails` provides RSpec feature helper methods that allow users to select or unselect options from both single and multiple chosen elements. Add the following to your `spec/rails_helper.rb` (or `spec/spec_helper.rb`):

```ruby
require 'chosen-rails/rspec'
```

This automatically configures RSpec by adding:

```ruby
RSpec.configure do |config|
  config.include Chosen::Rspec::FeatureHelpers, type: :feature
end
```

Configuration includes two additional methods for all `type: :feature` specs:

```ruby
chosen_select(value, options = {})
chosen_unselect(value, options = {})
```

Both methods require `from: '...'` inside the `options` hash that is either a CSS selector or a field's label (requires the `for` attribute on the label!).

### Example usage
To handle a single select:

```ruby
chosen_select('Leo Tolstoy', from: 'Author')
chosen_unselect('Leo Tolstoy', from: '#author')
chosen_select('Fyodor Dostoyevsky', from: '#author')
```

To handle a multiple select:

```ruby
chosen_select('Leo Tolstoy', 'Fyodor Dostoyevsky', 'Anton Chekhov', from: 'Authors')
# or, by single value:
chosen_select('Leo Tolstoy', from: '#authors')

chosen_unselect('Fyodor Dostoyevsky', ' Anton Chekhov', from: 'Authors')
# or, by single value:
chosen_unselect('Leo Tolstoy', from: '#authors')
```

## Gem maintenance

Maintain `chosen-rails` gem with `Rake` commands.

Update origin chosen source files.

```bash
rake update-chosen
```

Publish gem.

```bash
rake release
```

## License

use MIT license.
