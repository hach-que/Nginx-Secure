Nginx Secure Templates
============================

This is a framework for setting up isolated virtual hosts with Nginx.  This 
can be used when you want to ensure that each website can't reach another
(obviously you will also need to configure your filesystem ACLs so that
UNIX users are prevented from doing so).

In addition, if /usr/sbin/nginx-<name> exists, it will use this Nginx to
start that specific host.  This technique can be used to further limit
individual hosts with AppArmor or SELinux.  It is recommended that you
create this executable as a hardlink to the original using `ln`.

The two important files are `servers` and `php-enabled`.  These specify the
server ports, names and hostnames as well as what sites have PHP enabled.

Take a look at the Python script `setup` for how the configuration is
generated.

Installation
----------------

Copy everything in this directory apart from README.md into `/etc`.  The
systemd scripts are written so that when Nginx is restarted or reloaded
the `setup` script is automatically run to generate the latest set of
configuration files.  If you're not using systemd, it's up to you to
write scripts appropriate for whatever init system you are using.

Passthrough Servers
---------------------

If you have a paster or other non-uWSGI compatible server running on the
local machine, you can specify a passthrough server by omitting a
configuration file in the `vhosts.d` directory for the respective 
server name.  With this setup, the script will not produce a back-end
Nginx server to run on that port and will instead assume there is already
a HTTP server running on that port.
