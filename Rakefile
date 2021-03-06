#!/usr/bin/env rake
require 'yaml'

desc "Build Jekyll site"
task :build do
  exit 1 unless system "bundle exec jekyll build"
end

desc "Verify generated HTML"
task :verify_html do
  exit 2 unless system "bundle exec htmlproofer ./_site"
end

desc "Verify event data"
task :verify_data do
  data_files = [:past, :current]
  events = data_files.map { |file| YAML.load File.read "_data/#{file}.yml" }.flatten

  required_keys = ["dates", "name", "location"]
  missing_keys_error = false
  for event in events
    missing_keys = required_keys - event.keys
    unless missing_keys.empty?
      puts "Missing keys: #{missing_keys}"
      puts event
      missing_keys_error = true
    end
  end

  exit 3 if missing_keys_error

  dates = events.map { |event| Date.parse event["dates"].gsub(/[-&][^,]+/, '') }
  exit 4 unless dates.sort == dates
end

task default: [:build, :verify_html, :verify_data]
