# Troubleshoot

<p align="justify">
Troubleshooting and debugging is very imporatn in any complex IT solution. Hear in copper there are few critical points which is requied special attention in any case of technical issue. Basically copper solution consists with 5 major components . Email server, Openldap server, Group Office web client, Phpldapadmin and mysql databae. But Trouble shooting is mostly requied for emailserver and openldap server.
So it is requied to know the trouble shooting points of these two containers.
</p>

## Testing Postfix

Check the service is running

    service postfix status

Check the configuration

    postconf -n

Check the log

    more /var/log/mail.log

    more /var/log/mail.err

Change postfix configuration file and reload the configuration.

    postfix reload
    postfix/postfix-script: refreshing the Postfix mail system

### Enable/Disable plaintext Authentication for SMTP

Please comment out lines below in Postfix config file /etc/postfix/main.cf and reload or restart Postfix service:

    smtpd_sasl_auth_enable = yes
    smtpd_sasl_security_options = noanonymous

force all clients to use secure connection through port 25
    smtpd_tls_auth_only=yes

### Test mail sending with port 25 (plaintext authenticaiton)

Suppose your emailserver is in the localhost

    wso2s-MacBook-Pro:docker wso2$ telnet localhost 25
    Trying ::1…
    telnet: connect to address ::1: Connection refused
    Trying 127.0.0.1…
    Connected to localhost.
    Escape character is ‘^]’.
    220 mail.lsf.cu.lk ESMTP Postfix (Ubuntu)
    EHLO localhost
    250-mail.lsf.cu.lk
    250-PIPELINING
    250-SIZE 10240000
    250-VRFY
    250-ETRN
    250-STARTTLS
    250-ENHANCEDSTATUSCODES
    250–8BITMIME
    250-DSN
    250 SMTPUTF8
    MAIL FROM:info@lsf.cu.lk
    250 2.1.0 Ok
    data
    554 5.5.1 Error: no valid recipients
    subject:hello
    221 2.7.0 Error: I can break rules, too. Goodbye.
    Connection closed by foreign host.
    wso2s-MacBook-Pro:docker wso2$ telnet localhost 25
    Trying ::1…
    telnet: connect to address ::1: Connection refused
    Trying 127.0.0.1…
    Connected to localhost.
    Escape character is ‘^]’.
    220 mail.lsf.cu.lk ESMTP Postfix (Ubuntu)
    MAIL FROM:dadmin@lsf.cu.lk
    250 2.1.0 Ok
    RCPT TO:info@lsf.cu.lk
    250 2.1.5 Ok
    data
    354 End data with <CR><LF>.<CR><LF>
    subject:hello
    body:hello
    .
    250 2.0.0 Ok: queued as DBE312003D5
    421 4.4.2 mail.lsf.cu.lk Error: timeout exceeded
    Connection closed by foreign host.
    wso2s-MacBook-Pro:docker wso2$


### Test send mail with TLS (587 port)

Even though it is possible to send messages using port 25 configuration, it is not possible to send email with TLS activated port 587.
Then you have to check email server connection with TLS. You should have installed openssl tool.

```
root@email-b6c7d4c7f-5t87l:/var/log# openssl s_client -connect email:587 -starttls smtp
CONNECTED(00000003)
depth=0 C = US, ST = Oregon, L = Portland, O = lsf, OU = Org, CN = mail.copper.test.lk
verify error:num=18:self signed certificate
verify return:1
depth=0 C = US, ST = Oregon, L = Portland, O = lsf, OU = Org, CN = mail.copper.test.lk
verify return:1
---
Certificate chain
 0 s:/C=US/ST=Oregon/L=Portland/O=lsf/OU=Org/CN=mail.copper.test.lk
   i:/C=US/ST=Oregon/L=Portland/O=lsf/OU=Org/CN=mail.copper.test.lk
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIFrDCCA5SgAwIBAgIJAN+f6wFnoARaMA0GCSqGSIb3DQEBCwUAMGsxCzAJBgNV
1kcNTDFiDeY0F8m/L4C9wUyYA5Ebi5tJGc/Gz4AcAFcgF1Kxt9I3//rQl8Kp0X6D
djw8jiiXJMI82MGgUvJqYvYzAXwYin7aed+7PDkbC7hljlnJXEsnAMBROvQ++zHX
1dAf5AxeMj+YDFm6kiX/lg==
-----END CERTIFICATE-----
subject=/C=US/ST=Oregon/L=Portland/O=lsf/OU=Org/CN=mail.copper.test.lk
issuer=/C=US/ST=Oregon/L=Portland/O=lsf/OU=Org/CN=mail.copper.test.lk
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 2559 bytes and written 302 bytes
Verification error: self signed certificate
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 4096 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 971757A8240C0495552B360CCFACF8A2FBF00411A5B30605100F19E54D1722E9
    Session-ID-ctx: 
    Master-Key: 9380516DC2C86E8E67EABDB62A256DBEE2290A24D515646008C854DB737BB2AEA3C62E35AC6C484EB5A71814B1ED8AC4
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 5e 8b 3a 7a 5f 7f 2f 21-9a 8c e4 44 0f 08 28 dd   ^.:z_./!...D..(.
    0010 - bb fe 97 6d b8 04 2f 6c-4e 4a d9 86 b5 34 8e ee   ...m../lNJ...4..
    0020 - 86 df f3 f8 b4 98 9d 67-62 00 54 94 59 57 ad 48   .......gb.T.YW.H
    0030 - 58 50 d8 66 72 c3 04 e7-cb 8e 74 0a db da e6 9b   XP.fr.....t.....
    0040 - 06 d6 d3 db f2 6e f0 32-e0 a0 18 a0 d4 db d6 f1   .....n.2........
    0050 - 4d 97 fc 51 03 2e bb 20-1e 52 4e c6 41 34 38 32   M..Q... .RN.A482
    0060 - fb d9 7a 8d 01 af a5 45-5c 69 9a 93 87 fc af 27   ..z....E\i.....'
    0070 - e1 97 0b 77 f1 04 dd 15-65 15 3a 0a d3 dd bb 3c   ...w....e.:....<
    0080 - 5f de a4 23 4b 3e 4b 2a-d0 41 12 f4 04 83 7c 3b   _..#K>K*.A....|;
    0090 - a0 51 11 2b 71 69 a5 d1-f8 b6 26 51 2e fe 2b 1c   .Q.+qi....&Q..+.
    00a0 - 84 e9 6c 74 59 4c 46 11-a5 a9 fe c0 f1 b4 b2 46   ..ltYLF........F

    Start Time: 1553159047
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: yes
---
250 SMTPUTF8
mail from:test@copper.test.lk
250 2.1.0 Ok
rcpt to:test@copper.test.lk
554 5.7.1 <unknown[10.1.0.1]>: Client host rejected: Access denied
quit
221 2.0.0 Bye
closed
root@email-b6c7d4c7f-5t87l:/var/log# 

```

### Step 2 Check the submission port.

```
grep submission /etc/services
```

### Step 3 Check the postfix log file
```

Mar 21 08:47:22 email-b6c7d4c7f-5t87l postfix/submission/smtpd[395]: connect from unknown[10.1.7.188]
Mar 21 08:47:22 email-b6c7d4c7f-5t87l postfix/submission/smtpd[395]: NOQUEUE: reject: RCPT from unknown[10.1.7.188]: 554 5.7.1 <unknown[10.1.7.188]>: Client host rejected: Access denied; from=<test@copper.test.lk> to=<test@copper.test.lk> proto=ESMTP helo=<localhost>
Mar 21 08:47:22 email-b6c7d4c7f-5t87l postfix/submission/smtpd[395]: disconnect from unknown[10.1.7.188] ehlo=2 starttls=1 mail=1 rcpt=0/1 data=0/1 quit=1 commands=5/7
Mar 21 08:50:42 email-b6c7d4c7f-5t87l postfix/anvil[398]: statistics: max connection rate 1/60s for (submission:10.1.7.188) at Mar 21 08:47:22
Mar 21 08:50:42 email-b6c7d4c7f-5t87l postfix/anvil[398]: statistics: max connection count 1 for (submission:10.1.7.188) at Mar 21 08:47:22
Mar 21 08:50:42 email-b6c7d4c7f-5t87l postfix/anvil[398]: statistics: max cache size 1 at Mar 21 08:47:22


```

### Step 4 check the dovecot log file

```

Mar 21 08:48:28 imap-login: Info: Login: user=<test>, method=PLAIN, rip=10.1.7.188, lip=10.1.7.185, mpid=404, TLS, session=<pX9RzZaECtYKAQe8>
Mar 21 08:48:28 imap(test): Info: Logged out in=54 out=768
Mar 21 08:50:02 auth: Error: ldap_unbind
Mar 21 08:50:02 auth: Error: ldap_free_connection 1 1
Mar 21 08:50:02 auth: Error: ldap_send_unbind
Mar 21 08:50:02 auth: Error: ldap_free_connection: actually freed
```

### Step 5
According to observation looks like postfix has rejected client requests when comes from the port 587.
Then checked the submission has enabled in master.cf file. It should have following configuration.
```
submission inet n       -       n       -       -       smtpd
  -o syslog_name=postfix/submission
  -o smtpd_tls_security_level=encrypt
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_client_restrictions=permit_sasl_authenticated,reject
  -o milter_macro_daemon_name=ORIGINATING
```

### Step 5
Now checked mail sending with port number 25.
```
openssl s_client -connect email:25 -starttls smtp

    0090 - a1 d2 a7 4d e8 ed 1f 79-b7 8c ba a9 a3 0f 7a 2c   ...M...y......z,
    00a0 - 67 2b af f8 44 fc 61 c2-7f a9 7f 79 af fa e7      g+..D.a....y...
    00b0 - <SPACES/NULS>

    Start Time: 1553160437
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: yes
---
250 SMTPUTF8
mail from:test@copper.test.lk
250 2.1.0 Ok
rcpt to:test@copper.test.lk
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
subject:Test from 25
body:test form 25
.
250 2.0.0 Ok: queued as A8C1C3812E0
```

It was also success. Then it is clear issue comes only when sending mail using port number 587.

### check the submission port

```
grep submission /etc/services
```

### Email related issues and solutions.
```
https://support.plesk.com/hc/en-us/sections/115000422433-Mail
```

***



## Testing dovecot

check dovecot version

    dovecot — version
    2.2.22 (fe789d2)

check the active configuration

    doveconf -n


Check dovecot log files

    more /var/log/dovecot.log

Check dovecot service is running

    service dovecot status


### Disable/Enable plaintext unencripted authentication for DOVECOT

If you want to enable POP3/IMAP services without STARTTLS for some reason (again, not recommended), please update below two parameters in Dovecot config file /etc/dovecot/dovecot.conf and restart Dovecot service:

    on Linux and OpenBSD, it’s /etc/dovecot/dovecot.conf
    on FreeBSD, it’s /usr/local/etc/dovecot/dovecot.conf

    disable_plaintext_auth=no
    ssl=yes

Again, it’s strongly recommended to use only POP3S/IMAPS for better security.

    disable_plaintext_auth=yes
    ssl=required

Test by telnet

    Telnet <IP address of Client Access Server (Exchange)> <Port # 143>.

if connection not established

1. check ports
2. check 10-main.conf file whether 143 open for imap
3. check dovcot.conf wether listn ipv4

### Check user validaiton with dovecot imap server

    root@mail:/var/log# doveadm user thara@coppermail.dyndns.org
    field valuedoveadm(thara@coppermail.dyndns.org): Error: user thara@coppermail.dyndns.org: Auth USER lookup failed

    * then check configurations
    10-auth.conf
    and

### Test imap server (dovecot) with telnet

    root@mail:/etc/dovecot/conf.d# telnet localhost 143
    Trying 127.0.0.1…
    Connected to localhost.
    Escape character is ‘^]’.
    * OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE AUTH=PLAIN AUTH=LOGIN] Dovecot ready.
    01 login thara@coppermail.dyndns.org postfix@123
    01 OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS THREAD=ORDEREDSUBJECT MULTIAPPEND URL-PARTIAL CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS BINARY MOVE QUOTA] Logged in
    . list “” “*”
    * LIST (\HasNoChildren \Drafts) “.” Drafts
    * LIST (\HasNoChildren \Junk) “.” Junk
    * LIST (\HasNoChildren \Sent) “.” Sent
    * LIST (\HasNoChildren \Trash) “.” Trash
    * LIST (\HasNoChildren \Junk) “.” Spam
    * LIST (\HasNoChildren \Archive) “.” Archive
    * LIST (\HasNoChildren) “.” INBOX
    . OK List completed (0.001 + 0.000 secs).
    ? select inbox
    * FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
    * OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
    * 0 EXISTS
    * 0 RECENT
    * OK [UIDVALIDITY 1528974656] UIDs valid
    * OK [UIDNEXT 1] Predicted next UID
    ? OK [READ-WRITE] Select completed (0.008 + 0.000 + 0.007 secs).
    ?fetch 1 body[text]
    ?fetch BAD Error in IMAP command 1: Unknown command (4261338.063 + 0.000 secs).
    ?fetch 1 header[text]
    ?fetch BAD Error in IMAP command 1: Unknown command (4261381.185 + 0.000 secs).

    ? logout
    * BYE Logging out
    ? OK Logout completed (0.001 + 0.000 secs).
    Connection closed by foreign host.

## Testing openldap server

### Testing ldap server connectivity.
The OpenLDAP tools require that you specify an authentication method and a server location for each operation. To specify the server, use the -H flag followed by the protocol and network location of the server in question.

For basic, unencrypted communication, the protocol scheme will be ldap:// like this:

    ldapsearch -H ldap://server_domain_or_IP . . .

If you are communicating with a local server, you can leave off the server domain name or IP address (you still need to specify the scheme).

### Use ldif file for changing/updating the openldap server

It is possible to update or change openldap server from command line as explain bellow. 
For example how to add organizational unit using sample.ldif.

sample.ldif : 

    dn: ou=users,dc=coppermail,dc=dyndns,dc=org
    changetype: add
    objectClass: organizationalUnit
    objectclass: top
    ou: users

Command to insert a organization unit from coammand line.

```
ldapmodify -H ldap:// -x -D “cn=admin,dc=coppermail,dc=dyndns,dc=org” -w 12345 -f sample.ldif
```

### Test the openLDAP server

    ldapwhoami -H ldap:// -x

above command will check openldap server communication with unsecured parth.

You should now be able to upgrade your connections to use STARTTLS by passing the -Z option when using the OpenLDAP utilities. You can force STARTTLS upgrade by passing it twice. Test this by typing:

    ldapwhoami -H ldap:// -x -ZZ

This forces a STARTTLS upgrade. If this is successful, you should see:

    STARTTLS success

    anonymous

If you mis-configured something, you will likely see an error like this:

    STARTTLS failure

    ldap_start_tls: Connect error (-11)
    additional info: (unknown error code)

### Advanced TLS - ldap testing

Command “ldapsearch” with debug level 5, it seems succeed in Executing START TLS, and the ldap server’s certificate is valid,but it shows some other strange errors:

    ldapsearch -ZZ -d 5

If you want to troubleshoot whole TLS communication with ldap server step by step use bellow coammand.

    ldapsearch -x -ZZ -d1


## Database Testing

    Now a groupoffice database should be created preor to start groupoffice installation begins.

    first you have to check what is the pod name of dataase.

    #kubectl get pods -n  monitoring

    Then exicute followng command to open a mysql client connected with the mysql pod.

    #kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -n monitoring -- mysql -h mysql -pc0pperDB

    Then you will get access to the mysql database.
    Once you connected check databae.

        SHOW DATABASES;
        USE copper;
        SHOW TABLES;