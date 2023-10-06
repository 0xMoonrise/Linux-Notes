These are some things I used at some point, in case they are useful in the future
```bash
# These commands process a raw_data.txt file.
# They replace newline characters, pipes, and tabs with spaces,
# then replace multiple consecutive spaces with a single space,
# and finally split the content into lines after every 32-character hexadecimal string.
cat raw_data.txt | tr '\n|\t' ' ' | sed 's/\s\+/ /g' | sed "s/[0-9A-F]\{32\}/\n&/2g" ; echo

# This command replaces spaces with colons in the raw_data.txt file and saves it as data.txt.
cat raw_data.txt | sed "s: :\::" > data.txt

# The following commands are some commands that were used at some point.
# They create a tar.gz file from a folder, excluding certain subdirectories.
tar cvfz wildfly-10.1.tar /opt/wildfly-10.1.0.Final/ --exclude="/opt/wildfly-10.1.0.Final/standalone/log/" --exclude="/opt/wildfly-10.1.0.Final/standalone/tmp"

# Start the WildFly server in different modes.
/bin/bash /opt/wildfly-10.1.0.Final/bin/standalone.sh -c standalone-full.xml -b {ipaddr} -bmanagement {ipaddr} &
/bin/bash /opt/wildfly-10.1.0.Final/bin/standalone.sh -b {ipaddr} &

# Set environment variables related to Java.
JAVA_HOME=/usr/java/jdk1.8.0_151
JRE_HOME=$JAVA_HOME/jre
PATH=$JAVA_HOME/bin:$PATH:$HOME/bin

# Install Java OpenJDK 1.8 and perform some configurations.
dnf install java-1.8.0-openjdk
chown jboss.jboss -R /opt/wildfly-10.1.0.Final

# Configure the firewall to allow traffic on port 5432.
firewall-cmd --zone=public --add-port=5432/tcp --permanent
firewall-cmd --zone=public --permanent --add-port=5432/tcp
firewall-cmd --reload

# Show the allowed ports in the public zone of the firewall.
firewall-cmd --zone=public --list-ports

# Show the allowed ports again.
firewall-cmd --zone=public --list-ports

# Reload the PostgreSQL service and 
systemctl reload postgresql-10.service

# Install SNMP-related tools.
dnf install net-snmp net-snmp-libs net-snmp-utils

# Edit the SSH service configuration file and restart the service.
nano /etc/ssh/sshd_config && systemctl restart sshd.service
```