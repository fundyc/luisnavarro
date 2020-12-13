# My blog using Flexible-Jekyll

Taking advantage of a vacation I decided to create my own blog with Jekyll using the theme Flexible-Jekyll and this is the result.

## How to running it in local

### Prerequisites

#### Install Ruby (2.5 or higher)
```
brew install ruby
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile
```

#### Install Jekyll (in this case with 2.7 ruby version)
```
gem install --user-install bundler jekyll
echo 'export PATH="$HOME/.gem/ruby/2.7.0/bin:$PATH"' >> ~/.bash_profile
gem install jekyll bundler
```

### Run in local
```
bundle exec jekyll serve
```