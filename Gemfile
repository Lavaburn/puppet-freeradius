source "https://rubygems.org"

group :test do
	gem "metadata-json-lint"
		
	gem "puppet", ENV['PUPPET_VERSION'] || '~> 4.8.1'
	gem "puppetlabs_spec_helper"
end

group :integration do	
	# Version pinning for older Ruby versions	
	if RUBY_VERSION < '2.0.0'	
		gem 'beaker', '~> 2.52'
	else 
		gem 'beaker', '~> 3.13.0' # rubocop:disable Bundler/DuplicatedGem
	end
	
	gem "vagrant-wrapper"

	gem 'beaker-puppet_install_helper', '~> 0.7.1'
	gem 'beaker-rspec'
end

group :development do
    gem "travis"
    gem "travis-lint"
    gem "puppet-blacksmith"
    
	gem "guard-rake" if RUBY_VERSION >= '2.2.5'
end
