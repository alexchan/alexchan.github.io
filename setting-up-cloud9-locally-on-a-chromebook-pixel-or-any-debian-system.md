Title: Setting up Cloud9 locally on a Chromebook Pixel (or any debian system)
Date: 2013-06-19 05:07
Author: alex
Category: Technology
Slug: setting-up-cloud9-locally-on-a-chromebook-pixel-or-any-debian-system

BACKGROUND
----------

I received a Chromebook Pixel from Google I/O and although the screen is
absolutely beautiful, Chrome OS itself is somewhat limited.  I've set up
Crouton from the excellent instructions located
here: <https://github.com/dnschneid/crouton>.  I could have run Ubuntu
in  chroot but it doesn't quite look or feel the same.  I also wanted to
keep my system light so I've opted only for crouton's command line tools
instead of starting the window manager.  Without a window manager, it's
difficult to get an IDE working and after some research, I settled on
Cloud9 IDE.  Below is my experience setting it up.

REQUIREMENTS
------------

First, run the following to install the necessary Ubuntu packages:

    sudo apt-get install -y build-essential g++ curl libssl-dev apache2-utils git libxml2-dev

Next, it's very important to install the correct node version.  I
installed from the Ubuntu package manager which doesn't work well.  I
also tried the latest version which also doesn't work well and also a
different version from other tutorials.  I received different errors
during the "npm install" step.  The best way to do this is:

    curl https://raw.github.com/creationix/nvm/master/install.sh | sh

Logout and then log back in and execute the following:

    nvm install 0.8.8
    nvm alias default 0.8.8

At this point, you should have satisfied Cloud9's requirements.

INSTALLATION
------------

Installation is now pretty straight-forward.  You can follow the
directions on their Github (<https://github.com/ajaxorg/cloud9/>) page
or run the following:

    git clone https://github.com/ajaxorg/cloud9.git
    cd cloud9
    npm install

The above install steps create a `cloud9` directory with
a `bin/cloud9.sh` script that can be used to start Cloud9:

    bin/cloud9.sh

Optionally, you may specify the directory you'd like to edit:

    bin/cloud9.sh -w ~/git/myproject

Cloud9 will be started as a web server on port `-p 3131`, you can access
it by pointing your browser
to:[http://localhost:3131](http://localhost:3131/)

Enjoy your new web IDE!

