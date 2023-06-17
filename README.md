# Ghidra Without YAJSW

YAJSW is sinful. I don't like it. It arguably makes Ghidra Server unstable.

So, let's get rid of it!


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
