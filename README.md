# graylog
Install instruction

Graylog

Install the Graylog repository configuration and Graylog with the following command.
$ wget https://packages.graylog2.org/repo/packages/graylog-3.3-repository_latest.deb
$ sudo dpkg -i graylog-3.3-repository_latest.deb
$ sudo apt-get update && sudo apt-get install graylog-server graylog-enterprise-plugins graylog-integrations-plugins graylog-enterprise-integrations-plugins

Edit the graylog configuration file (/etc/graylog/server/server.conf).
$ sudo vi /etc/graylog/server/server.conf

Create you root_password_sha2 run the following command. Enter password and enter then it will generate password > Copy it and past on “root_password_sha2”
$ echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1

The root_password_sha2 configuration should look like below,
In configuration file should look like this for root_password_sha2
    # You MUST specify a hash password for the root user (which you only need to initially set up the
# system and in case you lose connectivity to your authentication backend)
# This password cannot be changed using the API or via the web interface. If you need to change it,
# modify it in this file.
# Create one by using for example: echo -n yourpassword | shasum -a 256
# and put the resulting hash value into the following line
root_password_sha2 = 3c11ba3cadf03068582144669023297adfb322f7b6348277f89267fb94908da8

Create password_secret run the following command.
$ pwgen -N 1 -s 96

In configuration file should look like this for password_secret
# You MUST set a secret to secure/pepper the stored user passwords here. Use at least 64 characters.
# Generate one by using for example: pwgen -N 1 -s 96:# ATTENTION: This value must be the same on all Graylog nodes in the cluster.
# Changing this value after installation will render all user sessions and encrypted values in the database invalid. (e.g. encrypted access t>
password_secret = eAJm49Oz9vTHA4ylv6MXA5sz9nCYp303Rq6lzywKPQToGINnopsjguh8nPot8iMDVEFVU4TNqpiJMoOuDeMTmXZBSwHURWLw

Set http_bind_address to the public host name or public IP address of the machine.
###############
# HTTP settings
###############

#### HTTP bind address
#
# The network interface used by the Graylog HTTP interface.
#
# This network interface must be accessible by all Graylog nodes in the cluster and by all clients
# using the Graylog web interface.
#
# If the port is omitted, Graylog will use port 9000 by default.
#
# Default: 127.0.0.1:9000
http_bind_address = 192.168.1.111:9000
#http_bind_address = [2001:db8::1]:9000

Enable Graylog startup with operating system and verify it is running.
$ sudo systemctl daemon-reload
$ sudo systemctl enable graylog-server.service
$ sudo systemctl start graylog-server.service
$ sudo systemctl --type=service --state=active | grep graylog

Set timezone on graylog server for admin user Edit.
 (sudo vi /etc/graylog/server/server.conf), 
<#Default is UTC root_timezone = Asia/Bangkok>
# The email address of the root user.
# Default is empty
#root_email = ""

# The time zone setting of the root user. See http://www.joda.org/joda-time/timezones.html for a list of valid time zones.
# Default is UTC
root_timezone = Asia/Bangkok

