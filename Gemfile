source "https://rubygems.org"

# Primary
gem "github-pages", "~> 231", group: :jekyll_plugins

# Theme
gem "just-the-docs", "~> 0.8.1"

# Plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-redirect-from", "~> 0.16.0"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# These break everything
# gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
# gem 'wdm', '>= 0.1.0' if Gem.win_platform?

gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
gem "webrick", "~> 1.8"
