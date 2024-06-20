# osquery-config
osquery best config 


OSQUERY installation Guide 

1- before installation 

########################----Disable Auditd


$ sudo  systemctl mask --now systemd-journald-audit.socket
$ sudo  systemctl disable auditd.service 



########################----Edit bashrc or bash.bashrc 

$ sudo  vi /etc/bashrc 
  
OR 

$ sudo  vi /etc/bash.bashrc

##########----add the end of file 

PROMPT_COMMAND= ”history -a”


2- installation 

#########################---Install Osquery
go to  official web site and download last version of osquery 

https://www.osquery.io/downloads/official/

#######------ install on Debian base


$sudo dpkg -i osquery_last-version.linux_amd64.deb


#######------  Install on Suse and RedHat Distro


$sudo rpm -i osquery-laset-version.linux.x86_64.rpm

###############################################################


3- config Osquery 

Download github osquery-config and go to osquery-config-main 

##################### -- copy config file

copy  osquery.conf  osquery.flags   extensions.load  to  /etc/osquery by this command


$ sudo   cp  extensions.load  osquery.*   /etc/osquery/

$ sudo  cp packs/*   /opt/osquery/share/osquery/packs/

$ sudo  vi /etc/rsyslog.d/osquery.conf

template(
  name="OsqueryCsvFormat"
  type="string"
  string="%timestamp:::date-rfc3339,csv%,%hostname:::csv%,%syslogseverity:::csv%,%syslogfacility-text:::csv%,%syslogtag:::csv%,%msg:::csv%\n"
)
*.* action(type="ompipe" Pipe="/var/osquery/syslog_pipe" template="OsqueryCsvFormat")

#### save it and exit 

#########################--- Rsyslog restart service 

$ sudo chmod 7633 /var/osquery/syslog_pipe

$sudo systemctl restart  rsyslog.service


#########################--- start OSQUERY 

$ sudo systemctl start osqueryd.service

$sudo systemctl enable osqueryd.service

#####    (Only Suse distro) 
$ sudo chkconfig osqueryd on  
############

$sudo systemctl status osqueryd.service
















