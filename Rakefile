require 'base64'

GFWLIST_URL = 'https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt'
ROOT_DIR    = File.expand_path('..', __FILE__)
SRC_FILE    = "#{ROOT_DIR}/dist/gfwlist.src.txt"
DIST_FILE   = "#{ROOT_DIR}/dist/gfwlist.txt"
SURGE_FILE  = "#{ROOT_DIR}/dist/surge.rules.txt"

desc 'Generate New List'
task :gen do
  cmd = "curl -k -s #{GFWLIST_URL} | base64 -d"
  rules = `#{cmd}`.strip.split("\n").select do |line|
    !(line =~ /^@@.*google.*/) &&
    !(line =~ /^@@.*gstatic.*/)
  end.join("\n")

  Dir["#{ROOT_DIR}/src/custom.list.d/*.list"].each do |f|
    rules += "\n\n#{File.read(f)}"
  end

  File.write(SRC_FILE, rules)
  File.write(DIST_FILE, Base64.encode64(rules))

  Rake::Task["surge:gen"].invoke
end

namespace :surge do
  desc 'Generate Surge Rules'
  task :gen do
    output = []
    File.readlines(SRC_FILE).each do |line|
      next if line =~ /^\[.*\]$/  # tag
      next if line =~ /^!/        # comments
      next if line.strip.empty?   # blank line

      if match = line.match(/^\|\|(.*)/)
        output << "DOMAIN-SUFFIX, #{match[1]}, Proxy"
      elsif match = line.match(/^@@\|\|(.*)/)
        output << "DOMAIN-SUFFIX, #{match[1]}, DIRECT"
      elsif match = line.match(/^([^@|].*)/)
        output << "DOMAIN-KEYWORD, #{match[1]}, Proxy"
      end

      File.write(SURGE_FILE, output.join("\n"))
    end
  end
end
