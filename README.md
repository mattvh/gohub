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


## Example Deployment Script

This is the script I use to deploy [Jekyll Themes.](http://jekyllthemes.org/) (It's really nice, since I can review a pull request, merge it in-browser, and have the changes instantly go live.)

    #!/bin/bash -l
    # encoding: utf-8
    
    GIT_REPO=https://github.com/redwallhp/matt.harzewski.com.git
    TMP_GIT_CLONE=/root/tmp/git/matt.harzewski.com
    PUBLIC_WWW=/var/www/sites/matt/blog
    
    export LC_ALL=en_US.UTF-8
    export LANG=en_US.UTF-8
    export LANGUAGE=en_US.UTF-8
    
    git clone $GIT_REPO $TMP_GIT_CLONE
    jekyll build --source $TMP_GIT_CLONE --destination $PUBLIC_WWW
    rm -Rf $TMP_GIT_CLONE
    exit


## License

This Software is licensed under the MIT License.

Copyright (c) 2012 adeven GmbH, http://www.adeven.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.