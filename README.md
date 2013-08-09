GoHub
=====

## What is GoHub?

GoHub is a little webserver written in go, [originally by adeven.](https://github.com/adeven/gohub) This fork is a little something I put together for my own usage. I moved the configuration and logs to /etc/gohub, made an Upstart script as well as a Bash installer script that builds the Go binary and copies everything to their proper places.

You can find further information about GoHub in the [original repository,](https://github.com/adeven/gohub) and in [this blog post]((http://big-elephants.com/2013-01/using-github-webhooks-for-deployment/)) by the author.


## Setup

First, clone the repository and run the setup script. This will build the binary and copy files to their proper locations.

**Note:** You must have [Go](http://golang.org/) installed fist.

    git clone https://github.com/redwallhp/gohub.git
    cd gohub
    chmod +x ./setup
    sudo ./setup

Second, add your repositories to the config.json file, which should now be found in `/etc/gohub`. "Repo" should be the name of your repository (without the username), and "Shell" is the shell script that will be run when GitHub hits the webhook. In most cases, you would keep them in `/etc/gohub/scripts`. You could also use an inline Bash command. (Be sure to `chmod +x` your scripts!)

    {
        "Hooks":[
            {
                "Repo": "yourawesomerepo",
                "Branch": "master",
                "Shell": "scripts/yourawesomerepo"
            }
        ]
    }

Finally, fire GoHub up through Upstart.

    service gohub start

Of course, you still need to add the webhook to your repository on GitHub. The URL shoould be formed like this:

    http://example.org:8392/reponame_branch
    http://myawesomeexamplesite.net:8392/myrepo_master
