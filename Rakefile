require 'open3'

task :prod do
  sh "bundle exec jekyll build"

  prod_dir="../prod.sal.martirano.net/"
  if Dir.exist?(prod_dir)
    puts "Using production directory: #{prod_dir}\n"
  else
    puts "EXITING - Production directory does not exist: #{prod_dir}\n" 
    exit 
  end

  branch_cmd = "cd #{prod_dir} && git branch | cut -b 3-6"
  stdout, stderr, status = Open3.capture3(branch_cmd)
  if status.success? 
    puts "#{branch_cmd} \n#{stdout}"
  else
    puts "EXITING - No success status.\n#{branch_cmd}\n#{stderr}\n#{stdout}\n" 
    exit 
  end
  branch=stdout.strip
  if branch!="prod"
    puts "EXITING - Not on prod" 
    exit 
  end

  rm_cmd = "cd #{prod_dir} && git rm -r *"
  stdout, stderr, status = Open3.capture3(rm_cmd)
  if status.success? 
    puts "#{rm_cmd} \n#{stdout}"
  else
    puts "EXITING - No success status.\n#{rm_cmd}\n#{stderr}\n#{stdout}\n" 
    exit 
  end

  cp_cmd = "cp -r _site/* #{prod_dir} && cd #{prod_dir} && git add * && git commit -m \"Auto update of prod files with _site files\" && git push dreamhost prod "
  stdout, stderr, status = Open3.capture3(cp_cmd)
  if status.success? 
    puts "#{cp_cmd} \n#{stdout}"
  else
    puts "EXITING - No success status.\n#{cp_cmd}\n#{stderr}\n#{stdout}\n" 
    exit 
  end
end
