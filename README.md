# Install procedures

For installing Gitorious in latest Ubuntu 11.04 x86_64 stable using Opscode Chef for a completely automated process, follow these instructions:

    sudo -i
    apt-get update
    echo "gem: --no-rdoc --no-ri" > /etc/gemrc
    apt-get install -y ruby ruby-dev libruby build-essential ssl-cert git
    cd /tmp
    wget http://production.cf.rubygems.org/rubygems/rubygems-1.4.2.tgz
    tar zxf rubygems-1.4.2.tgz
    ruby rubygems-1.4.2/setup.rb --no-format-executable
    gem install chef

    mkdir /etc/chef /root/chef-solo
    wget -O /etc/chef/solo.rb https://gist.github.com/raw/847256/chef-gitorious-etc-solo.rb
    wget -O /root/chef-solo/node.json https://gist.github.com/raw/847256/chef-gitorious-node-debian.json

First review the settings under /root/chef-solo/node.json. TODO: currently GMail is not supported as smtp relay server. Then procede with:

    cd /root/chef-solo
    git clone git://github.com/cessationoftime/gitorious-cookbooks.git cookbooks

    chef-solo

# Troubleshoot

If you have any problems, please fill the issue [here](https://github.com/rosenfeld/gitorious-cookbooks/issues).

1)

    if you are already using git and you have a group named "git" it will need to be renamed before running chef
    
2)
    make sure mysql is setup locally with the gitorious user and schema
    
If a gem fails to install properly, such as the one from: https://github.com/roman/rots.git, run the following and try running chef again

    cd /var/www/gitorious
    rm -rf vendor/cache

If for some reason apache is not listening in port 443 after install, please restart apache manually:

    invoke-rc.d apache2 restart

I have no idea why this happened to me once...

# Validate

Run the tests to make sure your installation is setup correctly!

    cd /var/www/gitorious
    bundle exec rake test