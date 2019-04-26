# Features
<p align="justify">
Copper Email Solution has been integrated many industry leading applicaitons to ensure most featurefull and secured email solution is delevered. Knowing each tool it was used will be good approach to know the solution well.
</p>

## MTA (Mail Transfer Agent ) - POSTFIX 
<p align="justify">
Postfix has been selected as the MTA in copper solution due to some of following advantages we found in postfix. To allow the server to send external emails, an MTA such as Sendmail, Postfix, or Exim is required. Mail is read either through direct access (shell login) or mailbox protocols like POP and IMAP. 
Postfix If you compare mail servers, then Postfix should be included, too, because it is also a popular mail server. It is a better mail server because it supports most of the current innovations in a modern-day mail server including but not limited to: • Virtual domains • SMTP relay • Consultation of SMTP client on its whitelist, grey list and blacklist databases • Host and user masquerading • Delivery to mailboxes with Maildir format As you compare mail servers, you will observe that Postfix utilises two large monolithic configuration files instead of the multiple task-oriented configuration files. You will then have to learn a single set of configuration file keywords. So we selected postfix considering It's popularity in integration projects and adaptability with new featues too.
</p>

Simple SMTP server.[POSTFIX](http://www.postfix.org).

## MDA (Mail Delevery Agent) - DOVECOT

<p align="justify">
Dovecot is an open source IMAP and POP3 email server for Linux/UNIX-like systems, written with security primarily in mind. Dovecot is an excellent choice for both small and large installations. It's fast, simple to set up, requires no special administration and it uses very little memory.
</p>

1. Best performing
2. Standards compliant
3. Clustered filesystems compatibilty
4. Flexible authentification
5. postfix and exim supprt for smtp authentication
6. Security

 Flexible IMAP server.[DOVECOT](https://www.dovecot.org).

## SPAM FILTER - RSPAMD

<p align="justify">
Rspamd is an advanced spam filtering system that allows evaluation of messages by a number of rules including regular expressions, statistical analysis and custom services such as URL black lists. Each message is analysed by Rspamd and given a spam score.

According to this spam score and the user’s settings, Rspamd recommends an action for the MTA to apply to the message, for example, to pass, reject or add a header. Rspamd is designed to process hundreds of messages per second simultaneously, and provides a number of useful features.
</p>

Here is the list of C modules available bundled with rspamd.

1. chartable: checks character sets of text parts in messages.
2. dkim: performs DKIM signatures checks.
3. fuzzy_check: checks messages fuzzy hashes against public blacklists.
4. spf: checks SPF records for messages processed.
5. surbl: this module extracts URLs from messages and check them against public DNS black lists to filter messages with malicious URLs.
6. regexp: the core module that allow to define regexp rules, rspamd internal functions and lua rules.

Lua modules

Lua modules are dynamically loaded on rspamd startup and are reloaded on rspamd reconfiguration. Should you want to write a lua module consult with the Lua API documentation. To define path to lua modules there is a special section named modules in rspamd:

    modules {
        path = "/path/to/dir/";
        path = "/path/to/module.lua";
        path = "$PLUGINSDIR/lua";
    }

If a path is a directory then rspamd scans it for `*.lua” pattern and load all files matched.

The following Lua modules are enabled in the default configuration (but may require additional configuration to work, see notes below):

1. antivirus - integrates virus scanners (requires configuration)
2. arc - checks and signs ARC signatures
3. asn - looks up ASN-related information
etc ******

Fast, free and open-source spam filtering.[RSPAMD](https://rspamd.com)

Comparisson with other spam filters. [SPAM FILTERS](https://rspamd.com/comparison.html)

## SPF - Sender Policy Framework 

<p align="justify">
Sender Policy Framework (SPF) is an email authentication method designed to detect forged sender addresses in emails (email spoofing), a technique often used in phishing and email spam.

SPF allows the receiver to check that an email claiming to come from a specific domain comes from an IP address authorized by that domain's administrators.[1] The list of authorized sending hosts and IP addresses for a domain is published in the DNS records for that domain. 
</p>

1. Authorize email senders with SPF
2. Help prevent spoofing from your domain


Set up SPF to prevent spammers from sending unauthorized emails from your domain. This type of spamming is called spoofing. Sender Policy Framework (SPF) is an email security method to prevent spoofing from your domain. 

Spoofing is a common unauthorized use of email, so some email servers require SPF. If you don't set up SPF for your domain, messages could bounce or could be marked as spam.
Use SPF with DKIM and DMARC

Along with SPF,  we recommend setting up DomainKeys Identified Mail (DKIM) and Domain-based Message Authentication, Reporting & Conformance (DMARC). SPF validates the domains that can send messages. DKIM verifies that message content is authentic and not changed. DMARC specifies how your domain handles suspicious emails that it gets.

</p>

### Create an SPF record for your domain

<p align="justify">
An SPF record is a TXT record that lists the mail servers that are allowed to send email from your domain. Messages sent from a server that isn't the SPF record might be marked as spam. SPF originally used TXT records in DNS, which are supposed to be free-form text with no semantics attached. SPF proponents readily acknowledge that it would be better to have records specifically designated for SPF, but this choice was made to enable rapid implementation of SPF. In July 2005, IANA assigned the Resource Record type 99 to SPF. Later on, the use of SPF records was discontinued, and as of 2017, it is still necessary to use TXT records.
    
</p>

Fast, free and open-source spam filtering.[SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework)

Create TXT Record in DNS for SPF . [TXT Record Creation](https://mediatemple.net/community/products/dv/204404314/how-can-i-create-an-spf-record-for-my-domain)