# frozen_string_literal: true

# kettle-jem:freeze
# To retain chunks of comments & code during kettle-jem templating:
# Wrap custom sections with freeze markers (e.g., as above and below this comment chunk).
# kettle-jem will then preserve content between those markers across template runs.
# kettle-jem:unfreeze

# activerecord-transactionable Rakefile v7.1.0 - 2026-08-02
# Ruby 2.3 (Safe Navigation) or higher required
#
# See LICENSE.md for license information.
#
# Copyright (c) 2026 Peter H. Boling (galtzo.com)
#
# Expected to work in any project that uses Bundler.
#
# Sets up tasks for appraisal2, floss_funding, kettle-jem, kettle-dev, rspec, minitest, rubocop_gradual, reek, yard, and stone_checksums.
#
# rake appraisal:install                      # Install Appraisal gemfiles (initial setup...
# rake appraisal:reset                        # Delete Appraisal lockfiles (gemfiles/*.gemfile.lock)
# rake appraisal:generate                     # Generate Appraisal gemfiles without resolving...
# rake appraisal:update                       # Generate and update Appraisal gemfiles
# rake bench                                  # Run all benchmarks (alias for bench:run)
# rake bench:list                             # List available benchmark scripts
# rake bench:run                              # Run all benchmark scripts (skips on CI)
# rake build:generate_checksums               # Generate both SHA256 & SHA512 checksums i...
# rake bundle:audit:check                     # Checks the Gemfile.lock for insecure depe...
# rake bundle:audit:update                    # Updates the bundler-audit vulnerability d...
# rake ci:act[opt]                            # Run 'act' with a selected workflow
# rake coverage                               # Run specs w/ coverage and open results in...
# rake default                                # Default tasks aggregator
# rake install                                # Build and install activerecord-transactionable-1.0.0.gem in...
# rake install:local                          # Build and install activerecord-transactionable-1.0.0.gem in...
# rake kettle:jem:install                     # Internal target used by `kettle-jem install`
# rake kettle:jem:selftest                    # Self-test: template activerecord-transactionable against itse...
# rake kettle:jem:template                    # Internal target used by scoped `kettle-jem template --only`
# rake reek                                   # Check for code smells
# rake reek:update                            # Run reek and store the output into the RE...
# rake release[remote]                        # Create tag v1.0.0 and build and push kett...
# rake rubocop_gradual                        # Run RuboCop Gradual
# rake rubocop_gradual:autocorrect            # Run RuboCop Gradual with autocorrect (onl...
# rake rubocop_gradual:autocorrect_all        # Run RuboCop Gradual with autocorrect (saf...
# rake rubocop_gradual:check                  # Run RuboCop Gradual to check the lock file
# rake rubocop_gradual:force_update           # Run RuboCop Gradual to force update the l...
# rake rubocop_gradual_debug                  # Run RuboCop Gradual
# rake rubocop_gradual_debug:autocorrect      # Run RuboCop Gradual with autocorrect (onl...
# rake rubocop_gradual_debug:autocorrect_all  # Run RuboCop Gradual with autocorrect (saf...
# rake rubocop_gradual_debug:check            # Run RuboCop Gradual to check the lock file
# rake rubocop_gradual_debug:force_update     # Run RuboCop Gradual to force update the l...
# rake spec                                   # Run RSpec code examples
# rake test                                   # Run tests
# rake yard                                   # Generate YARD Documentation
# rake yard:lint                              # Lint YARD Documentation
#

# simplecov:disable
require "bundler/gem_tasks" if !Dir[File.join(__dir__, "*.gemspec")].empty?
# simplecov:enable

# Define a base default task early so other files can enhance it.
desc "Default tasks aggregator"
task :default do
  puts "Default task complete."
end

# simplecov:disable
### MONOREPO FAMILY TASKS
if Dir.exist?(File.join(__dir__, "gems"))
  def family_gem_dirs
    Dir.glob(File.join(__dir__, "gems", "*", "*.gemspec"))
      .map { |path| File.dirname(path) }
      .uniq
      .sort_by { |path| File.basename(path) }
  end

  def run_kettle_family(*args)
    sh("bundle", "exec", "kettle-family", *args)
  end

  namespace :family do
    desc "List released Ruby subgems"
    task :list do
      family_gem_dirs.each { |path| puts File.basename(path) }
    end

    desc "Run release readiness checks for the Ruby gem family"
    task :readiness do
      run_kettle_family("check")
    end

    desc "Run tests for the Ruby gem family"
    task :test do
      run_kettle_family("test", "--execute")
    end

    desc "Run lint for the Ruby gem family"
    task :lint do
      run_kettle_family("lint", "--execute")
    end

    desc "Generate YARD docs for the Ruby gem family"
    task :docs do
      run_kettle_family("docs", "--execute")
    end

    desc "Report release state for the Ruby gem family"
    task :release_state do
      run_kettle_family("release-state")
    end

    desc "Run the Ruby gem family release planner"
    task :release do
      run_kettle_family("release")
    end

    desc "Execute the Ruby gem family release"
    task :release_execute do
      run_kettle_family("release", "--execute")
    end
  end
end
# simplecov:enable

# External gems that define tasks - add here!
begin
  require "kettle/dev"
  Kettle::Dev.install_tasks unless Kettle::Dev::RUNNING_AS == "rake"
rescue LoadError
  warn("NOTE: kettle-dev isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
end

### TEMPLATING TASKS
# These tasks are installed for the `kettle-jem` executable. Run full templating
# through `kettle-jem install`; use `kettle-jem template --only PATH` only for
# scoped file updates. The executable prepares the environment and then
# delegates here when rake orchestration is needed.
kettle_jem_selftest_unavailable_note = nil
begin
  require "kettle/jem"
  if Kettle::Jem.respond_to?(:install_tasks)
    Kettle::Jem.install_tasks
  else
    kettle_jem_selftest_unavailable_note = "NOTE: kettle-jem #{Kettle::Jem::Version::VERSION} does not provide rake tasks in this environment"
  end
rescue LoadError
  kettle_jem_selftest_unavailable_note = "NOTE: kettle-jem isn't installed, or is disabled for #{RUBY_VERSION} in the current environment"
end

if kettle_jem_selftest_unavailable_note
  desc("(stub) kettle:jem:selftest is unavailable")
  task("kettle:jem:selftest") do
    warn(kettle_jem_selftest_unavailable_note)
  end
end

# Galtzo FLOSS Rakefile v1.0.2 - 2025-08-12
# MIT License (see License.txt)
# Copyright (c) 2025 Peter H. Boling (galtzo.com)
# Sets up tasks for rspec, minitest, rubocop, reek, yard, and stone_checksums.
# rake build                            # Build my_gem-1.0.0.gem into the pkg directory
# rake build:checksum                   # Generate SHA512 checksum of my_gem-1.0.0.gem into the checksums directory
# rake build:generate_checksums         # Generate both SHA256 & SHA512 checksums into the checksums directory, and git commit them
# rake bundle:audit:check               # Checks the Gemfile.lock for insecure dependencies
# rake bundle:audit:update              # Updates the bundler-audit vulnerability database
# rake clean                            # Remove any temporary products
# rake clobber                          # Remove any generated files
# rake coverage                         # Run specs w/ coverage and open results in browser
# rake install                          # Build and install my_gem-1.0.0.gem into system gems
# rake install:local                    # Build and install my_gem-1.0.0.gem into system gems without network access
# rake reek                             # Check for code smells
# rake reek:update                      # Run reek and store the output into the REEK file
# rake release[remote]                  # Create tag v1.0.0 and build and push my_gem-1.0.0.gem to rubygems.org
# rake rubocop                          # alias rubocop task to rubocop_gradual
# rake rubocop_gradual                  # Run RuboCop Gradual
# rake rubocop_gradual:autocorrect      # Run RuboCop Gradual with autocorrect (only when it's safe)
# rake rubocop_gradual:autocorrect_all  # Run RuboCop Gradual with autocorrect (safe and unsafe)
# rake rubocop_gradual:check            # Run RuboCop Gradual to check the lock file
# rake rubocop_gradual:force_update     # Run RuboCop Gradual to force update the lock file
# rake spec                             # Run RSpec code examples
# rake test                             # Run tests / run spec task with test task
# rake yard                             # Generate YARD Documentation

defaults = []

is_ci = ENV.fetch("CI", "false").casecmp("true") == 0

### DEVELOPMENT TASKS
# Setup Kettle Soup Cover
begin
  require "kettle-soup-cover"

  Kettle::Soup::Cover.install_tasks
  # NOTE: Coverage on CI is configured independent of this task.
  #       This task is for local development, as it opens results in browser
  defaults << "coverage" unless Kettle::Soup::Cover::IS_CI
rescue LoadError
  desc("(stub) coverage is unavailable")
  task("coverage") do
    warn("NOTE: kettle-soup-cover isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

# Setup Bundle Audit
begin
  require "bundler/audit/task"

  Bundler::Audit::Task.new
  defaults.push("bundle:audit:update", "bundle:audit")
rescue LoadError
  desc("(stub) bundle:audit is unavailable")
  task("bundle:audit") do
    warn("NOTE: bundler-audit isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
  desc("(stub) bundle:audit:update is unavailable")
  task("bundle:audit:update") do
    warn("NOTE: bundler-audit isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

# Setup RSpec
begin
  require "rspec/core/rake_task"

  RSpec::Core::RakeTask.new(:spec)
  # This takes the place of `coverage` task when running as CI=true
  defaults << "spec" if !defined?(Kettle::Soup::Cover) || Kettle::Soup::Cover::IS_CI
rescue LoadError
  desc("spec task stub")
  task(:spec) do
    warn("NOTE: rspec isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

# Setup MiniTest
begin
  require "rake/testtask"

  Rake::TestTask.new(:test) do |t|
    t.test_files = FileList["tests/**/test_*.rb"]
  end
rescue LoadError
  desc("test task stub")
  task(:test) do
    warn("NOTE: minitest isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

# rubocop:disable Rake/DuplicateTask
if Rake::Task.task_defined?("spec") && !Rake::Task.task_defined?("test")
  desc "run spec task with test task"
  task test: :spec
elsif !Rake::Task.task_defined?("spec") && Rake::Task.task_defined?("test")
  desc "run test task with spec task"
  task spec: :test
else
  # Add spec as pre-requisite to 'test'
  Rake::Task[:test].enhance(["spec"])
end
# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# rubocop:enable Rake/DuplicateTask

# Setup RuboCop-LTS
begin
  require "rubocop/lts"

  Rubocop::Lts.install_tasks
  # Make autocorrect the default rubocop task
  defaults << "rubocop_gradual:autocorrect"
rescue LoadError
  desc("(stub) rubocop_gradual is unavailable")
  task(:rubocop_gradual) do
    warn("NOTE: rubocop-lts isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

# Setup Reek
begin
  require "reek/rake/task"

  Reek::Rake::Task.new do |t|
    t.fail_on_error = true
    t.verbose = false
    t.source_files = "{lib,spec,tests}/**/*.rb"
  end

  # Store current Reek output into REEK file
  require "open3"
  desc("Run reek and store the output into the REEK file")
  task("reek:update") do
    # Run via Bundler if available to ensure the right gem version is used
    cmd = [Gem.bindir ? File.join(Gem.bindir, "bundle") : "bundle", "exec", "reek"]

    output, status = Open3.capture2e(*cmd)

    File.write("REEK", output)

    # Mirror the failure semantics of the standard reek task
    unless status.success?
      abort("reek:update failed (reek reported smells). Output written to REEK")
    end
  end
  defaults << "reek:update" unless is_ci
rescue LoadError
  desc("(stub) reek is unavailable")
  task(:reek) do
    warn("NOTE: reek isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

# Setup Yard
begin
  require "yard"

  YARD::Rake::YardocTask.new(:yard) do |t|
    t.files = [
      # Source Splats (alphabetical)
      "lib/**/*.rb",
      "-", # source and extra docs are separated by "-"
      # Extra Files (alphabetical)
      "*.cff",
      "*.md",
      "*.txt",
      "REEK"
    ]
  end
  defaults << "yard"
rescue LoadError
  desc("(stub) yard is unavailable")
  task(:yard) do
    warn("NOTE: yard isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

### RELEASE TASKS
# Setup stone_checksums
begin
  require "stone_checksums"

  GemChecksums.install_tasks
rescue LoadError
  desc("(stub) build:generate_checksums is unavailable")
  task("build:generate_checksums") do
    warn("NOTE: stone_checksums isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
end

### DUPLICATE DRIFT TASKS
begin
  require "kettle/drift"
  Kettle::Drift.install_tasks
rescue LoadError
  desc("(stub) kettle:drift:check is unavailable")
  task("kettle:drift:check") do
    warn("NOTE: kettle-drift isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
  desc("(stub) kettle:drift:update is unavailable")
  task("kettle:drift:update") do
    warn("NOTE: kettle-drift isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
  desc("(stub) kettle:drift:force_update is unavailable")
  task("kettle:drift:force_update") do
    warn("NOTE: kettle-drift isn't installed, or is disabled for #{RUBY_VERSION} in the current environment")
  end
  desc("(stub) kettle:drift is unavailable")
  task("kettle:drift" => "kettle:drift:update")
end
