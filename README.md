# Awesome_Repo
This document is based on TACACS+ Authentication, and provides a detailed description on the new features for improve TACACS+ support.

SONiC currently supported TACACS+ features:

Authentication.
User session authorization.
User session accounting.
User command authorization with local permission.
New features:

User command authorization with TACACS+ server.
User command accounting with TACACS+ server.
1 Functional Requirement
1.1 User command authorization
Authorization when user run any executable file or script on SONiC host.

The full path and parameters will be sent to TACACS+ server side for authorization.
For recursive command/script, only the top level command have authorization.
No authorization for bash built-in command and bash function, but if a bash function call any executable file or script, those executable file or script will have authorization.
TACACS+ authorization is configurable:

TACACS+ authorization can be enable/disable.
TACACS+ authorization method will communicate with TACACS+ server for authorization, TACACS+ server should setup permit/deny rules.
Failover:

If a TACACS+ server not accessible, the next TACACS+ server authorization will be performed.
When all remote TACACS+ server not accessible, TACACS+ authorization will failed.
When TACACS+ authorization failed, fallover is configurable:
Disable local authorization as failover, then user can't run any command.
Enable local authorization as failover, then user can run command with local authorization.
For local authorization, please check TACACS+ Authentication.
When user login with local account, TACACS+ authentication and authorization will disabled for current user.
After login, user can run command with local authorization.
When all TACACS+ server not accessible, user can login with this method.
1.2 User command accounting
Accounting is the action of recording what a user is doing, and/or has done.

Following event will be accounted:

Command start event.
Command finish event.
User command inside docker container will not be accounted.

User command inside docker container actually run by docker service, so we can't identify if command are run by user or system service.
The 'docker exec ' command will be accounted because it's not run inside docker container.
Support TACACS+ accounting and local accounting:

TACACS+ will send event to TACACS+ server, and communication will be encrypted, for more detail please check RFC8907.
Local accounting will save event to syslog.
Both TACACS+ accounting and local accounting are configurable.
User secrets not exist in accounting event:

Use regex in /etc/sudoers PASSWD_CMDS to identify user secrets.
User secret will be replaced with ***.
1.3 User script
User can create and run their own script.
User may run script with interpreter commands:
python ./userscript.txt
sh ./usershellscript.txt
Allow user create and run script may cause potensial security issue, so suggest administrator setup rules from TACACS+ server side to block RO user create and run script.
1.4 Multiple TACACS server
Support config multiple TACACS server.
First server in the list will be primary server.
When a server not accessible, will try next server as backup.
When all server not accessible from SONiC, use native failover solution.
