# Prerequisites


## Host Requirment

Following requirments must be satisfied by the Email solution host, vertual machine or cloud environment.

RAM 2 GB

HDD 50 GB

Internet connectivity.


## Check Open Ports

Make sure any other application does not use ports that we are going to listen to

```
$ netstat -tulpn | grep -E -w '25|80|110|143|443|465|587|993|995|4190|11334'

//check each port

$ sudo lsof -i :25
```


Unblock following ports

| Service | Software | Protocol | Port |
| ------- | -------- | -------- | ---- |
| SMTP | Postfix | TCP | 25 |
| IMAP | Dovecot | TCP | 143 |
| SMTPS | Postfix | TCP | 465 |
| Submission | Postfix | TCP | 587 |
| IMAPS | Dovecot | TCP | 993 |


Then check external firewall also .


## Check Docker version

It is must to have docker installed in your host machine.

``` 