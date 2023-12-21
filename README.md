# Securing-device-access

## Enable Secret Password
```
Router(config)# enable algorithm-type ?
  md5     Encode the password using the MD5 algorithm
  scrypt  Encode the password using the SCRYPT hashing algorithm
  sha256  Encode the password using the PBKDF2 hashing algorithm


Router(config)# enable algorithm-type scrypt secret MySecret 
Router(config)# do show running-config | include enable
enable secret 9 $9$iZCzjXK/cNv5i1$F8HcrY9q.hjWt9J3WtFgxMppyvyX215hguwUq.UnRH2


Router(config)# enable secret class
Router(config)# do show running-config | include enable
enable secret 5 $1$dED2$iZBxzE61BHefpPO90hOUB/
```

## Line Password
```
Router(config)# line con 0
Router(config-line)# password cisco
Router(config-line)# login
Router(config-line)# exec-timeout 5 0
Router(config-line)# do show running-config | section line
line con 0
 password cisco                                                                          
 login
<...output omitted...>

Router(config)# service password-encryption
Router(config)# do show running-config | section line
line con 0
 password 7 1511021F0725                                                                 
<...output omitted...>
```

## Username Password
```
Router(config)# username badexample password badpassword
Router(config)# username admin privilege 15 secret Cisco123
Router(config)# username tier1 algorithm-type scrypt secret tier1secret
Router(config)# username tier2 algorithm-type sha256 secret tier2secret

Router(config)# line vty 0 4
Router(config-line)# login local
Router(config-line)# do show running-config | include username
username admin privilege 15 secret 5 $1$Cmuf$AUhxqg9U8vtmyXxnIvws90
username tier1 secret 9 $9$SDMCRTSdeALwnn$wCVReuYIqhfiJqdQ.o4SoxZL6pdTdodYgZBcVmqKyws
username tier2 secret 8 $8$fKF1lRle.c6i9X$NTeH9114hDNQpTB4ZN7yoJtcQcGYL0VSYn47bMK6nOE
username badexample password 7 011107004B0A151C36435C0D
```

## Additional configuration

Set minimum password length (Global configuration):
```
security passwords min-length 12
```
This command sets the minimum password length to 12 characters.

Allow only inbound SSH connections (vty lines):
```
line vty 0 4
transport input ssh
```
This command configures the first 5 vty lines (0-4) to only accept SSH connections.

Block logins after a specific number of failed login attempts (Global configuration):
```
login block-for 60 attempts 3 within 30
```
This command blocks login attempts for 60 seconds if there are 3 failed attempts within 30 seconds.

Allow only authorized devices to connect (Global configuration):
```
ip access-list standard ADMIN-ACCESS
permit host 192.0.2.1
deny any

login quiet-mode access-class ADMIN-ACCESS
```
This command creates an access list named "ADMIN-ACCESS" that only allows the host at 192.0.2.1, then applies that access list to the login quiet mode.

Specify delay between unsuccessful login attempts (Global configuration):
```
login delay 10
```
This command sets a delay of 10 seconds between unsuccessful login attempts.

