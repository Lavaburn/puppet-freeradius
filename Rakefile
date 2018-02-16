require 'puppetlabs_spec_helper/rake_tasks'

# These two gems aren't always present, for instance
# on Travis with --without development
begin
  require 'puppet_blacksmith/rake_tasks'
  Blacksmith::RakeTask.new do |t|
    t.tag_pattern = "v%s" # Use a custom pattern with git tag. %s is replaced with the version number.
  end
rescue LoadError
end

exclude_dirs = [
  "pkg/**/*",
  "spec/**/*",
  "vendor/**/*",
]

PuppetLint::RakeTask.new :lint do |config|
  config.ignore_paths = exclude_dirs
  config.disable_checks = [
    "80chars", "140chars", 
    "variable_is_lowercase", "class_inherits_from_params_class",
    "relative_classname_inclusion", "trailing_comma",
    "variable_contains_upcase", "version_comparison",
    "variable_is_lowercase", "arrow_on_right_operand_line"
  ] # TODO

  config.with_context = true
  config.relative = true
end

PuppetSyntax.exclude_paths = exclude_dirs

desc "Run syntax, lint, and spec tests."
task :test => [
  :syntax,
  :metadata_lint,
  :lint,
  :spec,
]

def io_popen(command)
  IO.popen(command) do |io|
    io.each do |line|
      print line
      yield line if block_given?
    end
  end
end

desc 'Vagrant VM power up and provision'
task :vagrant_up, [:manifest, :hostname] do |t, args|
  args.with_defaults(:manifest => 'init.pp', :hostname => '')
  Rake::Task['spec_prep'].execute
  ENV['VAGRANT_MANIFEST'] = args[:manifest]
  provision = false
  io_popen("vagrant up #{args[:hostname]}") do |line|
    provision = true if line =~ /is already running./
  end
  io_popen("vagrant provision #{args[:hostname]}") if provision
end

# Cleanup vagrant environment
desc 'Vagrant VM shutdown and fixtures cleanup'
task :vagrant_destroy do
  Rake::Task['spec_prep'].execute
  `vagrant destroy -f`
  Rake::Task['spec_clean'].execute
end
