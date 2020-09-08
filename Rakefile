require 'open3'

task :prod do
  sh "bundle exec jekyll build"

  prod_loc="../prod.sal.martirano.net/"

  branch_cmd = "cd #{prod_loc} && git branch | cut -b 3-6"
  stdout, stderr, status = Open3.capture3(branch_cmd)
  exit if !status.success? 
  branch=stdout.strip
  puts "Not on prod" && exit if branch!="prod"

  rm_cmd = "cd #{prod_loc} && git rm -r *"
  stdout, stderr, status = Open3.capture3(rm_cmd)
  exit if !status.success?

  cp_cmd = "cp -r _site/* #{prod_loc} && cd #{prod_loc} && git add *"
  stdout, stderr, status = Open3.capture3(cp_cmd)
  exit if !status.success?
end
