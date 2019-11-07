# Outlook Tips and Notes

## Outloop asks for password repeatedly

Below Commands will clean the sock and the outlook password problem is resolved.

1. Run CMD as admininstrator.
1. In the command line type: netsh winsock reset
1. Press Enter then restart your PC.

## Find SMPT server

The below command list the host names for SMTP servers

1. Open up a command prompt (CMD.exe)
1. Type nslookup and hit enter
1. Type set type=MX and hit enter
1. Type the domain name and hit enter, for example: google.com
