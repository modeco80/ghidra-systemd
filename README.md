# Ghidra Without YAJSW

YAJSW is sinful. I don't like it. It arguably makes Ghidra Server unstable.

So, let's get rid of it, and replace it with systemd!

# How to install

```
# useradd -mrU -s /sbin/nologin -d /srv/ghidra -k /dev/null ghidra # create "system" ghidra user
$ [editor] install # edit variables to match your setup
# ./install
[say yes to install and configure everything]
# systemctl enable --now ghidra
```

...

profit?

# Notes

- Java detection currently isn't implemented
	- The default is skewed for Arch Linux because it's what I run on everything, but altering to run on another distribution should Just Work:tm:

- You will need to install on a machine with direct public IP access
	- NAT doesn't play nicely with Ghidra Server yet
- `./svrAdmin` will require `sudo -u ghidra` to run (but should work fine besides that)
