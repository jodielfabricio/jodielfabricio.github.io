namespace :site do
  desc 'Sets up and sync the master branch where deploys are made'
  task :setup do
    if %x{ git branch } =~ /master/
      sh 'git branch -D master'
    end
    sh 'git checkout --orphan master'

    sh 'git rm -rf .'
    sh 'rm -rf .DS_Store source build'

    sh 'git fetch origin master'
    sh 'git pull origin master'
    sh 'git checkout source'
  end

  desc 'Builds and deploys a new version of the site'
  task :deploy do
    sh 'rm -rf build'
    sh 'jekyll build'
    sh 'mv build /tmp'

    sh 'git checkout master'
    sh 'rm -rf build source'
    sh 'git pull --rebase origin master'

    sh 'cp -r /tmp/build/* .'
    sh 'git add --all'
    sh "git commit -m 'Site build on #{date} at #{moment}'"
    sh 'git push origin master'

    sh 'rm -rf /tmp/build'
    sh 'git checkout source'
  end
end

def set_time
  @now ||= Time.now
end

def date
  set_time && @now.strftime('%d/%m/%y')
end

def moment
  set_time && @now.strftime('%H:%M')
end
