require 'base64'

GFWLIST_URL = 'https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt'
ROOT_DIR    = File.expand_path('..', __FILE__)
SRC_FILE    = "#{ROOT_DIR}/dist/gfwlist.src.txt"
DIST_FILE   = "#{ROOT_DIR}/dist/gfwlist.txt"

desc 'Generate new list'
task :gen do
  cmd = "curl -k -s #{GFWLIST_URL} | base64 -d"
  rules = `#{cmd}`.strip

  Dir["#{ROOT_DIR}/src/custom.list.d/*.list"].each do |f|
    rules += "\n\n#{File.read(f)}"
  end

  File.write(SRC_FILE, rules)
  File.write(DIST_FILE, Base64.encode64(rules))
end
