require 'bundler/gem_tasks'
$stdout.sync = true

begin
  require 'rspec/core/rake_task'
  RSpec::Core::RakeTask.new(:spec)
rescue LoadError
end

desc 'Open Shoryuken pry console'
task :console do
  require 'pry'
  require 'shoryuken'

  config_file = File.join File.expand_path('..', __FILE__), 'shoryuken.yml'

  if File.exist? config_file
    config = YAML.load File.read(config_file)

    Aws.config = config['aws']
  end

  def push(queue, message)
    Shoryuken::Client.queues(queue).send_message(message_body: message)
  end

  ARGV.clear
  Pry.start
end

desc 'Enqueue 1k messages'
task :enqueue_1k do
  require 'shoryuken'

  require File.join(File.expand_path('..', __FILE__), 'putsreq_worker')

  1000.times do |i|
    PutsReqWorker.perform_async(i.to_s)
    print '.'
  end
end
