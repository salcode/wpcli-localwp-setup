# SSH into Local By Flywheel

I spend most of my time working on the command line on my Mac.  When working with [Local by Flywheel](https://local.getflywheel.com/), I find it frustrating to go to the Local by Flywheel app and click a drop-down to SSH into the machine.

Instead, I'd prefer a command I can type from the command line that would:

- Based on the directory I'm currently in, SSH into the correct docker container.
- Based on the directory I'm currently in, within the docker container change to the corresponding directory.

## Script

The `sshlbf` (for SSH Local by Flywheel) does this task based on a file called `.dockerid` that needs to be manually created in the root directory for that particular site.  The `.dockerid` file should contain one line that has the container name for that site.

## Credits

[Sal Ferrarello](https://salferrarello.com) / [@salcode](https://twitter.com/salcode)
