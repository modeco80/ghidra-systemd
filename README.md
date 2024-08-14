# Ghidra Without YAJSW (systemd)

YAJSW is awful, I don't like it, and it arguably makes Ghidra Server a pain to run.

Therefore, these scripts provide the ability to replace it with systemd (enforcing best practices when running services in the process).

# Installation

Installation is relatively easy, and consists of a few key steps.

Command line excerpts follow Arch Linux "# means root/$ means user" documentation paragidm.

## Creating the service user

First, create a system `ghidra` user that will be used to run the server. 

This can be done with the following command:

`# useradd -mrU -s /sbin/nologin -d /srv/ghidra -k /dev/null ghidra` 

This creates a system `ghidra` user with the following properties:

- It's shell is set to nologin, so it can't be logged into (basically death insurance, since system users can't be logged into via normal means)
- The home directory for the user is `/srv/ghidra`
- The skeleton directory is set to `/dev/null`, disabling skeleton file copying (only the directory will be made, and we won't get a bunch of files that aren't needed for the service)

Note that the home directory can be anywhere, but this is what I've used and it works, so YMMV changing it (the script uses getpwent anyways like it should, so I don't think anything would go too wrong? But I'm not sure)

## Edit configuration

Next, use your favorite text editor to edit the variables in the `config.example` file to match your setup, and save it to `config`.

The variables you need to edit are:

- Path to `java` executable
- Ghidra installation path
- Ghidra anonymous read access allowed (This is opt-in, and Ghidra Server requires individual projects to opt in as well)
- (Optional) Listen IP address (the default uses `eth0.me` to figure it out, but if you have a desired interface, you can provide one)

## Running installation

After you've done that, you can now run the install script with `# ./install`.

It will display the configuration, and ask if it's OK to continue with the installation.

Upon reviewing, if the configuration is correct, type `yes` or `y`, and the installation will commence.

Once everything is installed, a `ghidra.service` unit is now available to launch your Ghidra server.

Additionally, at this point, you can now copy an existing repository folder into `/srv/ghidra/repositories` to migrate an existing server installation.

## Autostart

Once done, you can use

`# systemctl enable --now ghidra`

to enable (make the service automatically start at boot) and start your Ghidra server.

## Logs

Note that logs will be placed in to the systemd journal, and no log files will be updated or created when using the service. This is by design.

To look at logs for your Ghidra server, run

`$ journalctl -u ghidra` (or what have you)

## Administration

You will need to use `[your ghidra user's home]/svradmin` to launch the svrAdmin tool. 

You can also just use `sudo -u ghidra`, but the helper is nicer to use IMO

# Notes/Caveats

- Java path detection currently isn't implemented
	- The default is skewed for Arch Linux because it's what I run on everything, but you can change it

- You will need to install on a machine with direct public IP access
	- NAT doesn't play nicely with Ghidra Server yet
