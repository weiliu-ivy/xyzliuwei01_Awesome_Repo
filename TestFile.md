3 Limitation
3.1 Command size
TACACS protocol limitation: command + parameter size should smaller than 240 byte. The longer than 240 bytes parts will be drop.
This limitation is a protocol level, all TACACS implementation have this limitation, include CISCO, ARISTA and Cumulus.
Both Authorization and Accounting have this limitation.
When user user a command longer than 240 bytes, only commands within 240 bytes will send to TACACS server. which means Accounting may lost some user input. and Authorization check can only partly check user input.
3.2 Server count
Max TACACS server count was hardcoded, default count is 8.
3.3 Local authorization
Operation system limitation: SONiC based on Linux system, so permission to execute local command are managed by Linux file permission control. This means when enable both TACACS+ authorization and local authorization, local authorization will always happen after TACACS+ authorization.
3.4 Docker support
Any command run inside a shell in a docker container will not covered by Authorization and Accounting.
Docker exec command will be covered by Authorization and Accounting.
Administrator may setup TACACS+ rules to block docker exec command for RO user:
user can start an interactive shell on the docker container, then run command inside container to evade TACACS+ authorization and accounting.
3.5 Recursive commands
Many linux command allow user start a harmless process and run another command from it, administrator may setup TACACS+ rules from server side to block user from:
Run another shell.
Run interpreter, for example python.
Run loader, for example /lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
Run find/VI command which can run other commands inside it.
