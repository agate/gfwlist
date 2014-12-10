require 'base64'

GFWLIST_URL = 'https://autoproxy-gfwlist.googlecode.com/svn/trunk/gfwlist.txt'
ROOT_DIR    = File.expand_path('..', __FILE__)
DIST_FILE   = "#{ROOT_DIR}/dist/gfwlist.txt"

desc 'Generate new list'
task :gen do
  rules = `curl -s #{GFWLIST_URL} | base64 -d`.strip

  Dir["#{ROOT_DIR}/src/custom.list.d/*.list"].each do |f|
    rules += "\n#{File.read(f)}"
  end

  File.write(DIST_FILE, Base64.encode64(rules))
end
