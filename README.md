# Ghidra Without YAJSW

YAJSW is sinful. I don't like it. It arguably makes Ghidra Server unstable and hard to deal with.

So, let's get rid of it, and replace it with systemd (enforcing best practices when running services in the process)!

# How to install

Note that command line excerpts follow Arch Linux "# means root/$ means user" documentation paragidm.

First create a system `ghidra` user. This can be done with the following command

`# useradd -mrU -s /sbin/nologin -d /srv/ghidra -k /dev/null ghidra` 

This creates a system `ghidra` user with the following properties:

- its shell is set to nologin, so it can't be logged into (even if it was somehow exposed, since system users cant be logged into via SSH/etc anyways)
- the home directory is /srv/ghidra, with no additional skeleton files being placed (that's what the `-k /dev/null` option does)

Next, use your favorite text editor to edit the variables at the top of the ./install file to match your setup:

- Java path
- Ghidra installation directory
- Ghidra anonymous read access allowed (This is opt-in, and Ghidra Server requires individual projects to opt in as well)

After you've done that, you can now run

`# ./install`

It will prompt with the configuration, and ask if it's ok to continue. If so, type `yes` or `y`, and the installation will begin.

Once installed, a `ghidra.service` unit is now available to launch your Ghidra Server.

You can now copy an existing repository folder into `/srv/ghidra/repositories` at this point if migrating an existing server.

Once done, you can use

`# systemctl enable --now ghidra`

to enable (make the service start at boot) and start your Ghidra server.

Note that logs will be placed in to the systemd journal, and no log files will be updated when using the service. 

To look at logs for your Ghidra server, run

`$ journalctl -u ghidra`

# Notes

- Java path detection currently isn't implemented
	- The default is skewed for Arch Linux because it's what I run on everything, but you can change it

- You will need to install on a machine with direct public IP access
	- NAT doesn't play nicely with Ghidra Server yet
- `./svrAdmin` will require `sudo -u ghidra` to run (but should work fine besides that)
	- You could create a wrapper script that just consists of `sudo -u ghidra @ghidra_home@/server/svrAdmin $@` (i may do so)
