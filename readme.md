### ssh in a EC2 instance
    $ ssh -i [key] ubuntu@[public-ip]

### Install nvm
    $ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

The script clones the nvm repository to ~/.nvm, and attempts to add the source lines from the snippet below to the correct profile file (~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc).

    $ export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
 
#### Verify nvm Installation
    $ -v nvm

which should output nvm if the installation was successful. Please note that which nvm will not work, since nvm is a sourced shell function, not an executable binary.

### Install Node version:12
    $ nvm install 12

#### Verify Node Installation
    $ node -v

which will output v12.22.3

### Install Ghost
make an empty directory
    $ mkdir ghost

    $ npm install ghost-cli -g

    $ cd ghost

    $ ghost install local

output will be 'Ghost was installed successfully! To complete setup of your publication, visit:
http://localhost:2368/ghost/ '

ghost is running on the localhost of the server.

### Bind the port with Eth0 interface
Now, ghost is running on the LoopBack (lo) interface of the AWS server. if we want to expose port, we have to bind the port with Eth0 interface. so, go to the files
    $ ls
    output: config.development.json  content  current  versions

    $ vim config.development.json
    output will be

{
  "url": "http://localhost:2368/",
  "server": {
    "port": 2368,
    "host": "127.0.0.1"
  },
  "database": {
    "client": "sqlite3",
    "connection": {
      "filename": "/home/ubuntu/ghost/content/data/ghost-local.db"
    }
  },
  "mail": {
    "transport": "Direct"
  },
  "logging": {
    "transports": [
      "file",
      "stdout"
    ]
  },
  "process": "local",
  "paths": {
    "contentPath": "/home/ubuntu/ghost/content"
  }
}

we have to change the host to "0.0.0.0"

then stop and start the Ghost again
$ ghost stop
$ ghost start

and now, it is running on http://localhost:2368/ghost/ on the server and
http://[public-ip]:2368/ from any device
