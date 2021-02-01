## modify ruby resource
gem sources -l

gem sources --remove https://rubygems.org/

gem sources -a https://mirrors.ustc.edu.cn/rubygems/

gem install bundler -v 1.17.3
