Sample init scripts and service configuration for mobinoded
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/mobinoded.service:    systemd service unit configuration
    contrib/init/mobinoded.openrc:     OpenRC compatible SysV style init script
    contrib/init/mobinoded.openrcconf: OpenRC conf.d file
    contrib/init/mobinoded.conf:       Upstart service configuration file
    contrib/init/mobinoded.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "mobinode" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, mobinoded requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, mobinoded will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that mobinoded and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If mobinoded is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/mobinode/mobinode.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/mobinode.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/mobinoded
Configuration file:  /etc/mobinode/mobinode.conf
Data directory:      /var/lib/mobinoded
PID file:            /var/run/mobinoded/mobinoded.pid (OpenRC and Upstart)
                     /var/lib/mobinoded/mobinoded.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the mobinode user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
mobinode user and group.  Access to mobinode-cli and other mobinoded rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start mobinoded" and to enable for system startup run
"systemctl enable mobinoded"

4b) OpenRC

Rename mobinoded.openrc to mobinoded and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/mobinoded start" and configure it to run on startup with
"rc-update add mobinoded"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop mobinoded.conf in /etc/init.  Test by running "service mobinoded start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy mobinoded.init to /etc/init.d/mobinoded. Test by running "service mobinoded start".

Using this script, you can adjust the path and flags to the mobinoded program by
setting the MobinodeD and FLAGS environment variables in the file
/etc/sysconfig/mobinoded. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
