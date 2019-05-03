<!-- # Motivation
<p align="justify">
Organizations in Sri Lanka’s public sector, such as the Sri Lanka Army, need to own and operate their own email infrastructure as they are not in a position to use cloud services for such critical technology due to security, privacy and national independence concerns. This is a requirement not just for Sri Lanka but globally and is in fact not limited to the public sector.

While there are now high quality proprietary off the shelf systems available, it is not acceptable to simply buy such a solution for reasons of cost, independence and control.

While there are open source components for all parts of an email solution available, building the technical skill to put that together in a safe and secure operational manner requires significant technical skills that are not readily available in many public sector organizations.

This project’s objective is to provide a comprehensive email solution which can be readily deployed without complex configuration and which receives active maintenance and support from this project team. It is expected that each organization would deploy this solution for itself in their own data center or a shared data center, but this project team will provide the professional support and management needed to install and operate the system safely and securely to whatever extent the organization needs. This can eventually even go as far as managed hosting for the organization. The intent is to provide extremely limited customization and tuning capability to end user administrators - certainly no more than what Google provides for Google Apps customers.

The promotion and adoption of this solution by non public sector organizations is a secondary goal and interesting as a method of revenue generation. Our long term objective is to make this project self-funding in a not-for-profit manner.
</p> -->

# What is an “Email Solution”?
<p align="justify">
The components include the following:
</p>
<ul>
    <li> Core 
        <ul>
            <li> ESMTP server </li>
            <li> Scalable storage architecture for “cloud email” service experience and because public sector mail should be permanently archived (similar to the Google Vault service) </li>
            <li> Webmail server </li>
            <li> IMAP server </li>
            <li> Virus and malware scanning tool </li>
            <li> Spam filtering tool </li>
        </ul>
    </li>
    <li> Clients
        <ul>
            <li> Browser client </li>
            <li> Support for standard mobile clients for Android and iOS phones / tablets </li>
            <li> (Optionally) Support for standard desktop clients for Windows, Linux and MacOS </li>
        </ul>
    </li>
    <li> Mailing list / group system </li>
    <li> Support for LDAP and SSO
        <ul>
            <li> Integration to organization wide LDAP directory for authentication </li>
            <li> Web client integration to Single Sign On portal via SAML2 or OIDC </li>
        </ul>
    </li> 
    <li> Secure mail
        <ul>
            <li> Support for MAC mail encryption and signing with key management from identity solution. </li>
            <li> "Domain-based Message Authentication, Reporting & Conformance" protocol 
                <ul>
                    <li> Overview [https://dmarc.org/overview/](https://dmarc.org/overview/) </li>
                    <li> Setup [https://dmarcguide.globalcyberalliance.org/#/](https://dmarcguide.globalcyberalliance.org/#/) </li>
                </ul>
            </li>
        </ul>
    </li>
</ul>

## Global opportunity
<p align="justify">
While we are building this solution to solve a critical need in Sri Lanka, the same requirement exists in many countries.
</p>

## Refferences

Copper main repository is it's github repository [[Copper github](https://github.com/LSFLK/Copper)]
  

<!-- Prometheus container pull and run: 
    sudo docker pull prom/prometheus
    docker run -p 9090:9090 prom/prometheus

Grafana pull and run
    docker pull grafana/grafana
    docker run -d --name=grafana -p 3000:3000 grafana/grafana -->


## Contact us
    
If you have any questions or doubts about Cu, please [contact us](mailto:copper@opensource.lk)