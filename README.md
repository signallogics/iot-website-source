# Getting Started with Vagrant (recommended)
This currently does NOT work on Windows. See "Setting up on Windows" below.

In order to modify the IoT website you will need some software. We use
“vagrant” to create a virtual machine to run this software to ensure
that the correct versions are being used, regardless of platform and
platform version.

You will need to install “VirtualBox” and “vagrant” <sup><a id="fnr.1" class="footref" href="#fn.1">1</a></sup>

VirtualBox can be found at:
<https://www.virtualbox.org/wiki/Downloads>

Vagrant can be found at: <https://www.vagrantup.com/downloads.html>

Download the appropriate installers and run them.

Assuming you have a GitHub account, you should clone the website
source repository (assuming you are starting from your home directory)

Issue the following commands (The “#” and stuff after it is a comment,
don't type it in!):

    git clone git@github.com:mit-cml/iot-website-source
    cd iot-website-source
    vagrant up  # <– This step will take a *long* time, particularly
                #    the first time you run it.

At this point vagrant is downloading a ubuntu (linux) VM, starting it up
in VirtualBox and provisioning it, which means it is downloading the
software needed to build the website. This takes quite awhile, but only
the first time you do the “vagrant up” command. When complete, move on to *Building the site using Vagrant.*

# Building the site using Vagrant (recommended)

When you start working on the site for the day, you will begin by running `vagrant up` in the "iot-website-source" folder. Then you can log in to the VM:

    vagrant ssh # <– This will connect you to the virtual machine
                #    your next command will be in the context of the
                #    virtual machine

    cd /vagrant/docs
    python makepages.py
    npm start

This will compile the source of the website. It will be obvious if there
are errors. If all goes well, you can go to “localhost:3000” on your
host machine with a browser and see the site.

Once you are running, you are ready to edit. See *Where the pages are&#x2026;* below.

# Getting Started without Vagrant

You don't need vagrant if you have or can get node.js yourself. This means you're responsible for keeping your tools updated, as Vagrant isn't there to do it for you. This is probably much faster at initial setup than the Vagrant path.

Things you need:
* node.js: <https://nodejs.org/en/download/>
* python 2 (not 3): <https://www.python.org/downloads/>

Assuming you have a GitHub account, you should clone the website
source repository (assuming you are starting from your home directory)

Issue the following commands:

    git clone git@github.com:mit-cml/iot-website-source

    cd iot-website-source
    npm install
    cd docs
    npm install

Now you're ready!

# Building the site without Vagrant

When you start working on the site for the day, you will change into the docs directory and start npm.

    cd iot-website-source/docs
    npm start

This will compile the source of the website. It will be obvious if there are errors. If all goes well, you can go to “localhost:3000” on your host machine with a browser and see the site.

Once you are running, you are ready to edit. See *Where the pages are&#x2026;* below.

# Setting up on Windows (without Vagrant - sort of recommended)

Install [Github Desktop](https://github-windows.s3.amazonaws.com/GitHubSetup.exe), [Python 2.7](https://www.python.org/downloads/), and [node.js](https://nodejs.org/en/download/).

Add Python to your path variable:
- Go to My Computer :arrow_right: Properties :arrow_right: Advanced :arrow_right: Environment Variables
- Under User Variables, add `;C:\Python27` to the end of the user path Variables
- If there is no User Path variable, create it and add `C:\Python27`

Clone the repository using Github Desktop. The default location is `Documents\GitHub\iot-website-source`. Know where it went.

Open Powershell, change (cd) into the repository directory, from above. In the directory, run:
```
> npm install
> cd docs
> npm install
```
and that's it! You're ready for "Using on Windows Without Vagrant," below.

# Using on Windows Without Vagrant

There will be two powershell windows. One will run the server, the other will have a script that you re-run to apply any new changes.

In Powershell, change into the `iot-website-source\docs` directory, and run `python makepages.py` (if you use tab to autocomplete, it will be rewrite it to `python .\makepages.py` and that's fine.)

Open a second pwershell instance, change into the same directory and run `npm run browser:development`.

Open your browser and go to `localhost:3000`.

When you make changes to the files and want to see them in the browser, go to the first Powershell window and re-run `python makepages.py`. (You can use the up arrow on the keyboard so you don't have to re-type it.)


# Where the pages are&#x2026;

Page source (in Markdown) live in `docs/src/app/pages`. You can
edit any of the pages in that directory and rebuild the site and you
will see your changes.

If you are using vagrant, this will be inside the `/vagrant` folder.

If you are not using vagrant, this will be inside the `iot-website-source` folder.

If the processor is running (because you have run `npm start`), then the website in your brownser ("localhost:3000") will automatically update when you save changes to any of these files.

## Adding a new page

This is a little more complicated, but only a little.

Create your page in /vagrant/docs/src/app/pages. You then need to edit
the makepages.py script to assign your page a path. Look at the lines
that are there to see how to do it. You then need to edit
/vagrant/docs/src/app/AppNavDrawer.js to add your page to the
hierarchy of pages defined there. Again, look at the existing pages as
a guide.

**DO NOT EDIT /vagrant/docs/src/app/AppRoutes.js** This file is
 automatically generated by the makepages.py script (which is why you
 edit the script to add a page).

So far we have two top-level drawers defined, “Teachers” and
“Extensions”. You can add a new top-level draw. However to do so you
also need to edit /vagrant/docs/src/app/Master.js. Look around line
160 where the existing top level drawers are mentioned. Add your draw
there (using the code there as a guide).

When you are done, use the “npm start” to test out your additions.

# Building the site for public distribution

To build the site for upload to the distribution location you do:

    cd /vagrant/docs
    python makepages.py
    npm run browser:build

If all goes well you can type “exit” (or control-D) to leave the virtual
machine. You then do:

    vagrant halt

This will shutdown the virtual machine. You can then to a “vagrant up”
in the future to test/build the site again. It should be much faster
then the initial time.

## Updating the production site and deploying it

At this point you are going to clone (or “pull” if you already have a
copy) the production built website to your local machine. You will
then update the site from the files you built above and then you will
push it out to deploy it.

**IMPORTANT**: You will not be able to do this until you are authorized.
Send your “ssh” public key (**not** private key) to jis@mit.edu and I can
add you.

The details here are for the site as run from our own server at
<http://iot.appinventor.mit.edu>. These details will change when we
move to GitHub.

Assuming you do not have a copy of the site do (from your home
directory):

    git clone git@iot.appinventor.mit.edu:iot-pages
    cd iot-website-source/docs/build/
    rsync -v -a * ../../../iot-pages/
    cd ../src/www
    rsync -v -a blocks ../../../../iot-pages/
    cd ../../../../iot-pages
    git add . --all
    git commit # <– This will invoke an editor for you to put in the commit comment

This upates your local copy of the production site.

Now push it to production, which will update the deployed site:

    git push origin master





<div id="footnotes">
<h2 class="footnotes">Footnotes: </h2>
<div id="text-footnotes">

<div class="footdef"><sup><a id="fn.1" class="footnum" href="#fnr.1">1</a></sup> <div class="footpara">If you are running under Linux, you can avoid using VirtualBox and can
use “lxc” instead, which is much faster. See Jeff for details.</div></div>


</div>
</div>
