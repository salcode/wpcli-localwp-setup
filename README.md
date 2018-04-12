# SSH into Local By Flywheel

I spend most of my time working on the command line on my Mac.  When working with [Local by Flywheel](https://local.getflywheel.com/), I find it frustrating to go to the Local by Flywheel app and click a drop-down to SSH into the machine.

Instead, I'd prefer a command I can type from the command line that would:

- Based on the directory I'm currently in, SSH into the correct docker container.
- Based on the directory I'm currently in, within the docker container change to the corresponding directory.

## SSH Script

The `sshlbf` (for SSH Local by Flywheel) does this task based on a file called `.dockerid` that needs to be manually created in the root directory for that particular site.  The `.dockerid` file should contain one line that has the container name for that site.

## General Setup

### Download this Project

Copy this project to your local machine.  I use Git to make a copy of the project
in my home directory into a new directory called `ssh-into-local-by-flywheel`.

```
$ cd ~
$ git clone git@github.com:salcode/ssh-into-local-by-flywheel.git
```

### Add these Commands to Our Path

To use these new commands, `sshlbf` and `wpcli-lbf-setup`, we need to add our directory
to our PATH environment variable. If you're not familiar with PATH and environment
variables, I suggest reading this StackExchange entry
[What are PATH and other environment variables, and how can I set or use them?](https://superuser.com/q/284342).

We are going to add the following lines to `~/.bash_profile`.

```
# Prepend this directory to the PATH environment variable.
# See https://superuser.com/q/284342
export PATH=~/ssh-into-local-by-flywheel:$PATH
```

Then exit your terminal and start it once again.

Now if you type

```
$ sshlbf
```

you should get the message

> Failed to SSH in. No .dockerid file was found.

This is a good thing, as it means `sshlbf` is working properly.

Now we need to configure our individual sites.

## Per Site SSH Script Setup

The command `sshlbf` (for SSH Local By Flywheel) needs to know the Docker container name
in order to do its job.  To make this work we need to manually create a file called
`.dockerid` in the root directory *for each site* we want to use with this.
The `.dockerid` file will contain one line that has the container name for the site.

To create a `.dockerid` file.

### 1. SSH into the website

SSH into the website using the Local by Flywheel option `Open Site SSH`

### 2. Find the SSH Entry File

Take note of the string in the terminal, it should look something like

```
/Users/sal/Library/Application\ Support/Local\ by\ Flywheel/ssh-entry/HyjrkJ8eG.sh; exit
```
This tells use the path to the file Local by Flywheel uses to SSH us in.  This file
contains the Docker container name (i.e. the docker id).

### 3. Log Out of the SSH Session

You can **not** determine the docker ID when you are SSHed into Local by Flywheel.

Log out of your SSH session.  You can do this by typing `exit`.

### 4. View the File with the Docker ID

We want to view the file Local by Flywheel uses to SSH us in.  You can do this with
your favorite text editor or from the command line with something like.

```
$ less /Users/sal/Library/Application\ Support/Local\ by\ Flywheel/ssh-entry/HyjrkJ8eG.sh
```

Notice, I'm using the path we found in step 2.  Also, note I'm not including
the `; exit` at the end of the line.

### 5. Copy the Docker ID

When we view the file we should see something like

```
export DISABLE_AUTO_TITLE="true"
echo -n -e "\033]0;Iron Code Studio\007"
eval $("/Applications/Local by Flywheel.app/Contents/Resources/extraResources/virtual-machine/vendor/docker/osx/docker-machine" env local-by-flywheel);
"/Applications/Local by Flywheel.app/Contents/Resources/extraResources/virtual-machine/vendor/docker/osx/docker" exec -it 0301fc5e38236161287154e08385e30929dc718ba9e9160cf04e19c169ca4944 /bin/bash;
```

The long alpha-numeric string near the end is the Docker container name (Docker ID) that we want. Copy this value.

In my case it would be

```
0301fc5e38236161287154e08385e30929dc718ba9e9160cf04e19c169ca4944
```

### 6. Create the .dockerid file

In the project root directory, create a file called `.dockerid`.

This file should contain only one line and that should be the Docker container name we copied from above.

### 7. Use sshlbf

Now you should be able to run

```
$ sshlbf
```

from any directory in this website (e.g. from `/wp-content`) and be SSHed into
Local by Flywheel and moved to the corresponding directory.

## Setup WP CLI from Host Machine

Much of my command line work that requires SSHing into the machine specifically involves using [WP CLI](http://wp-cli.org/). Thanks to work by [Morgan Estes](https://github.com/morganestes), I found I could add some configuration files to my local project that would allow me to use WP CLI from my Mac on the WordPress site running inside Local by Flywheel.

### How to Setup

To setup WP CLI to work from the host machine (your Mac), you need to add `wp-cli.local.yml` and `wp-cli.local.php` to your project root directory (i.e. next to `app/`, `conf/`, and `logs/`). I've added a helper script to this project to do just this.

On your host machine (your Mac), navigate to the root directory of your project.  When you run `ls` you should see

```
app     conf    logs
```

Then run the helper script in this project

```
$ wpcli-lbf-setup
```

Then you'll be prompted for the **Database host** and **Database port**, both of these values can be found in the Local by Flywheel interface under your Site > DATABASE settings.

Then you should see a Success message,

```
Successfully connected to http://example.test
```

where `example.test` will be replaced by your local site URL.

### Using WP CLI

Now from your host machine (your Mac) you can enter a WP CLI command and it will be executed in the Local by Flywheel guest website.

```
$ wp plugin list
```

### Troubleshooting

If the script shows a failure message, try the following:

- Confirm the site is running in Local
- Double-check the database host and port

## Credits

[Sal Ferrarello](https://salferrarello.com) / [@salcode](https://twitter.com/salcode)
