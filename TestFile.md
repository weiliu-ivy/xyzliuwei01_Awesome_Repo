set testing  information

3 Limitation

3.1 Command size

TACACS protocol limitation: command + parameter size should smaller than 240 byte. The longer than 240 bytes parts will be drop.

This limitation is a protocol level, all TACACS implementation have this limitation, include CISCO, ARISTA and Cumulus.


Both Authorization and Accounting have this limitation.

When user user a command longer than 240 bytes, only commands within 240 bytes will send to TACACS server. which means Accounting may lost some user input. and Authorization check can only partly check user input.

3.2 Server count

Max TACACS server count was hardcoded, default count is 8.

3.3 Local authorization

