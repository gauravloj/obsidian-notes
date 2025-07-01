
Simple log viewer: https://github.com/sevdokimov/log-viewer





Logging frameworks:
* Common Event Expression (CEE): This standard, developed by MITRE, provides a common structure for log data, making it easier to generate, transmit, store, and analyse logs.
* OWASP Logging Cheat Sheet: A guide for developers on building application logging mechanisms, especially related to security logging.
* Syslog Protocol: Syslog is a standard for message logging, allowing separation of the software that generates messages from the system that stores them and the software that reports and analyses them.
* NIST Special Publication 800-92: This publication guides computer security log management.
* Azure Monitor Logs: Guidelines for log monitoring on Microsoft Azure.
* Google Cloud Logging: Guidelines for logging on the Google Cloud Platform (GCP).
* Oracle Cloud Infrastructure Logging: Guidelines for logging on the Oracle Cloud Infrastructure (OCI).
* Virginia Tech - Standard for Information Technology Logging: Sample log review and compliance guideline.


- `ntpdate pool.ntp.org` - synchronize system time


linux logging


Log forwarding:

Configure rsyslog to log all sshd messages to a specific file, such as /var/log/websrv-02/rsyslog_sshd.log. The steps below can be followed to achieve this:
1. Open a Terminal.
2. Ensure rsyslog is Installed: You can check if rsyslog is installed by running the command: sudo systemctl status rsyslog
3. Create a Configuration File: Use a text editor to create the following configuration file: gedit /etc/rsyslog.d/98-websrv-02-sshd.conf, nano /etc/rsyslog.d/98-websrv-02-sshd.conf, vi /etc/rsyslog.d/98-websrv-02-sshd.conf, or vim /etc/rsyslog.d/98-websrv-02-sshd.conf
4. Add the Configuration: Add the following lines in /etc/rsyslog.d/98-websrv-02-sshd.conf to direct the sshd messages to the specific log file: 
$FileCreateMode 0644
:programname, isequal, "sshd" /var/log/websrv-02/rsyslog_sshd.log
5. Save and Close the Configuration File.
6. Restart rsyslog: Apply the changes by restarting rsyslog with the command: sudo systemctl restart rsyslog
7. Verify the Configuration: You can verify the configuration works by initiating an SSH connection to localhost via ssh localhost or by checking the log file after a minute or two.

Another syslog config sample for cron job
# Log Forwarding with rsyslog
$FileCreateMode 0644
:programname, isequal, "CRON" /var/log/websrv-02/rsyslog_cron.log
# Forward Logs to SIEM-02:51514
# *.* @10.10.10.101:51514


Log rotation

Log Management with logrotate
This activity aims to introduce logrotate, a tool that automates log file rotation, compression, and management, ensuring that log files are handled systematically. It allows automatic rotation, compression, and removal of log files. As an example, here's how we can set it up for /var/log/websrv-02/rsyslog_sshd.log:
1. Create a Configuration File: sudo gedit /etc/logrotate.d/98-websrv-02_sshd.conf, sudo nano /etc/logrotate.d/98-websrv-02_sshd.conf, sudo vi /etc/logrotate.d/98-websrv-02_sshd.conf, or sudo vim /etc/logrotate.d/98-websrv-02_sshd.conf
2. Define Log Settings:
/var/log/websrv-02/rsyslog_sshd.log {
    daily
    rotate 30
    compress
    lastaction
        DATE=$(date +"%Y-%m-%d")
        echo "$(date)" >> "/var/log/websrv-02/hashes_"$DATE"_rsyslog_sshd.txt"
        for i in $(seq 1 30); do
            FILE="/var/log/websrv-02/rsyslog_sshd.log.$i.gz"
            if [ -f "$FILE" ]; then
                HASH=$(/usr/bin/sha256sum "$FILE" | awk '{ print $1 }')
                echo "rsyslog_sshd.log.$i.gz "$HASH"" >> "/var/log/websrv-02/hashes_"$DATE"_rsyslog_sshd.txt"
            fi
        done
        systemctl restart rsyslog
    endscript
}
3. Save and Close the file.
4. Manual Execution: sudo logrotate -f /etc/logrotate.d/98-websrv-02_sshd.conf


Parsing different logs:

# Process nginx access log
awk -F'[][]' '{print "[" $2 "]", "--- /var/log/gitlab/nginx/access.log ---", "\"" $0 "\""}' /var/log/gitlab/nginx/access.log  | sed "s/ +0000//g" > /tmp/parsed_consolidated.log

# Process rsyslog_cron.log
awk '{ original_line = $0; gsub(/ /, "/", $1); printf "[%s/%s/2023:%s] --- /var/log/websrv-02/rsyslog_cron.log --- \"%s\"\n", $2, $1, $3, original_line }' /var/log/websrv-02/rsyslog_cron.log >> /tmp/parsed_consolidated.log

# Process rsyslog_sshd.log
awk '{ original_line = $0; gsub(/ /, "/", $1); printf "[%s/%s/2023:%s] --- /var/log/websrv-02/rsyslog_sshd.log --- \"%s\"\n", $2, $1, $3, original_line }' /var/log/websrv-02/rsyslog_sshd.log >> /tmp/parsed_consolidated.log

# Process gitlab-rails/api_json.log
awk -F'"' '{timestamp = $4; converted = strftime("[%d/%b/%Y:%H:%M:%S]", mktime(substr(timestamp, 1, 4) " " substr(timestamp, 6, 2) " " substr(timestamp, 9, 2) " " substr(timestamp, 12, 2) " " substr(timestamp, 15, 2) " " substr(timestamp, 18, 2) " 0 0")); print converted, "--- /var/log/gitlab/gitlab-rails/api_json.log ---", "\""$0"\""}' /var/log/gitlab/gitlab-rails/api_json.log >> /tmp/parsed_consolidated.log



Log report

# Get the audit report
aureport --summary
aureport --failed


# ---- Search the audit ---- 

# returns successful logins
ausearch --message USER_LOGIN --success yes --interpret

# returns failed logins
ausearch --message USER_LOGIN --success no --interpret




windows logging

Log analysis tools: https://ericzimmerman.github.io/#!index.md


Event IDs

Logon events	Description
4624	A user successfully logged on to a computer.
4625	Logon failed due to an unknown username or a wrong password.
4634	The logoff process was completed.
4647	A user started the logoff process.
4779	A user disconnected from a remote session without logging off.

