# SSH into Local By Flywheel

I spend most of my time working on the command line on my Mac.  When working with [LocalWP](https://localwp.com/), I find it frustrating to go to the LocalWP app and click a drop-down to "Open Site Shell" in order to run a [WP CLI](https://wp-cli.org) command.

Instead, I'd prefer to be able to run a WP-CLI command from the command line inside the project (without the extra step).

This project includes a setup script to make this possible (`wpcli-localwp-setup`).

## General Setup

### Download this Project

Copy this project to your local machine.  I use Git to make a copy of the project
in my home directory into a new directory called `wpcli-localwp-setup`.

```
$ cd ~
$ git clone git@github.com:salcode/wpcli-localwp-setup.git
```

### Add these Commands to Our Path

To use the new commands `wpcli-localwp-setup`, we need to add our directory
to our PATH environment variable. If you're not familiar with PATH and environment
variables, I suggest reading this StackExchange entry
[What are PATH and other environment variables, and how can I set or use them?](https://superuser.com/q/284342).

We are going to add the following lines to `~/.zshrc`.

```
# Prepend this directory to the PATH environment variable.
# See https://superuser.com/q/284342
export PATH=~/wpcli-localwp-setup:$PATH
```

Then exit your terminal and start it once again.

### Setup for a LocalWP Site

To setup WP-CLI to work, you need to add `wp-cli.local.yml` and `wp-cli.local.php` to your project root directory (i.e. next to `app/`, `conf/`, and `logs/`). I've added a helper script to do just this.

Navigate to the root directory of your project.  When you run `ls` you should see

```
app     conf    logs
```

Then run the helper script in this project

```
$ wpcli-localwp-setup
```

Then you'll be prompted for the **Database socket**, which can be found in the LocalWP interface under your Site > DATABASE settings.

Then you should see a Success message,

```
Successfully connected to http://example.test
```

where `example.test` will be replaced by your local site URL.

### Using WP CLI

Now you can enter a WP-CLI command and it will be executed in the LocalWP website.

```
$ wp plugin list
```

### Troubleshooting

If the script shows a failure message, try the following:

- Confirm the site is running in LocalWP
- Double-check the database socket

## Credits

[Sal Ferrarello](https://salferrarello.com) / [@salcode](https://twitter.com/salcode)
