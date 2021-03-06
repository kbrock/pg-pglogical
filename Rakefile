require "bundler/gem_tasks"
require "rspec/core/rake_task"

def create_database(dbname)
  require "pg"
  c = PG::Connection.new(:dbname => "postgres")
  c.async_exec("CREATE DATABASE #{dbname}")
rescue PG::DuplicateDatabase => err
  raise unless err.message =~ /already exists/
end

def drop_database(dbname)
  require "pg"
  c = PG::Connection.new(:dbname => "postgres")
  c.async_exec("DROP DATABASE #{dbname}")
rescue PG::InvalidCatalogName => err
  raise unless err.message =~ /does not exist/
end

namespace :spec do
  desc "Setup the test databases"
  task :setup => :teardown do
    create_database("pg_pglogical_test")
    create_database("pg_pglogical_test_target")
  end

  desc "Teardown the test databases"
  task :teardown do
    drop_database("pg_pglogical_test")
    drop_database("pg_pglogical_test_target")
  end
end

RSpec::Core::RakeTask.new(:spec)
task :default => :spec
