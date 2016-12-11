desc "Clear out _deploy dir"
task :clear do
  puts "Clearing out existing _deploy files..."
  sh "rm -rf _deploy/*"
end

desc "Build the site before deployment"
task :build do
  puts "Building the site..."
  task("clear").invoke
  sh "JEKYLL_ENV=production bundle exec jekyll build"
end

desc "Serve the site locally"
task :serve do
  puts "Serving the site..."
  task("clear").invoke
  sh "bundle exec jekyll serve"
end

desc "Deploy the site"
task :serve do
  puts "Deploying the site..."
  task("clear").invoke
  sh "deployblog"
end
