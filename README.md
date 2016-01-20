This is a copy of what I built for getting a server set up to host a Ghost
blog, including the download, configuration, and running of each Ghost
instance on NGINX.

**Note:** This hasn't been well vetted for public consumption yet, but I wanted
to open it up in case it could be of use to anyone else. Please submit any
issues or pull requests if you see room for improvement!

# Features
- Installs Node.js and npm
- Downloads and extracts the latest stable version of Ghost
- Creates a separate Ghost installation for each site specified in `./group_vars/ghost.yaml`
- Configures an Upstart script for each site to recover if it goes down

# Requirements
- Ansible on your host machine
- A server with SSH access for the root user (if you want to change the SSH
  user, you can do so in `./group_vars/ghost.yaml`)

# Getting Started

1. Edit the following files:

  - `./group_vars/ghost.yaml`  
    Add each of your sites - along with a unique port number - to the `sites:` defition, following the template provided.
  - `ghost-config.js.j2`  
    This is the Ghost config that will be used for each site. The domain and 
  port will be inserted automatically from `group_vars/ghost.yaml`, but 
  you should set up mail and anything else you need.
  - `hosts`  
    Replace the example IP with the address of your server.
  - `vhost.conf`  
    This is the virtualhost config template that will be used for each site. Note the URL redirect at the bottom.

2. Next, run `./bin/server` to set up the necessary pieces on the server. You
should only have to do this once, but it is idempotent and can be run as many 
times as you need.

3. Run `./bin/sites` once to set up each of the sites specified in
`./group_vars/ghost.yaml`. As with `server`, you can run this
multiple times without ill effect. While it __won't__ touch
your content directory, making a backup is always a good
idea. :)

# Resources

Just some things I referenced while setting this up.

- [Ghost - Installing Ghost on Linux](http://support.ghost.org/installing-ghost-linux/)
- [Ansible - Intro to Playbooks](http://docs.ansible.com/playbooks_intro.html)
- [Ansible - nginx + WordPress Examples](https://github.com/ansible/ansible-examples/blob/master/wordpress-nginx/group_vars/all)

Finally, much :heart: to the Ghost team for creating this awesome platform, and to Ansible for being badass as always. :+1:
