This API documentation is based on the open-source Slate project. You can read the original readme [./README.md]

## Editing

To edit the API doc, simply modify [./source/index.html.md]

## Testing locally

### Installing

You should need to do this only once per machine.

#### Install Ruby

```ruby
brew install ruby23
```

**IMPORTANT**: Slate currently does not work with Ruby 2.4

#### Install dependencies

```ruby
bundle install
```

### Starting the server

```ruby
bundle exec middleman server
```

Your local docs should now be available at http://localhost:4567


### Deploying

Codeship will automatically deploy once you merge into the master branch
