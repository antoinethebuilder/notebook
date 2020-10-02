# Resources

## AWS

[Automating safe, hands-off deployments](https://aws.amazon.com/builders-library/automating-safe-hands-off-deployments/)  
[LocalStack - A fully functional local AWS cloud stack](https://github.com/localstack/localstack#localstack---a-fully-functional-local-aws-cloud-stack)

## DevOps

### [Bento by Chef](https://github.com/chef/bento)

Packer templates for building Vagrant base boxes. A subset of templates are built and published to the bento org on Vagrant Cloud. These published boxes serve as the default boxes for [kitchen-vagrant](https://github.com/test-kitchen/kitchen-vagrant/).

### [HuskyCI](https://github.com/globocom/huskyCI)

Open Source Tool that orchestrates security tests and centralizes all results into a database for further analysis and metrics.

### Elasticsearch

* [Generate certificates](https://www.elastic.co/guide/en/elasticsearch/reference/current/encrypting-communications-certificates.html#encrypting-communications-certificates)
* [Add nodes to your cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/encrypting-communications-hosts.html#encrypting-communications-hosts)
* [Elasticsearch detection rules](https://github.com/elastic/detection-rules)
* \*\*\*\*[Visualizing observability data](https://www.elastic.co/webinars/visualizing-observability-data)
* [Encrypting Internode communications](https://www.elastic.co/guide/en/elasticsearch/reference/current/encrypting-internode.html)
* [Ad hoc threat hunting with Elastic Security](https://www.elastic.co/webinars/ad-hoc-threat-hunting-with-elastic-security)
* [Federal Security Standards: Hardening World](https://www.elastic.co/webinars/federal-security-standards-hardening-world)
* [Instrument and monitor a Python application using APM](https://www.elastic.co/webinars/instrument-and-monitor-a-python-application-using-apm)
* [Alerting for observability, security, and the Elastic Stack](https://www.elastic.co/webinars/alerting-for-the-elastic-stack)
* [Putting anomalies into context with custom URLs in Kibana](https://www.elastic.co/blog/putting-anomalies-into-context-with-custom-urls-in-kibana) \(premium\)

## Cyber defense

### Web Applications

#### \*\*\*\*[**ModSecurity Core Rule Set \(CRS\)**](https://coreruleset.org/)\*\*\*\*

The CRS aims to protect web applications from a wide range of attacks, including the OWASP Top Ten, with a minimum of false alerts. The CRS provides protection against many common attack categories, including:

| SQL Injection \(SQLi\)  Cross Site Scripting \(XSS\)  Local File Inclusion \(LFI\)  Remote File Inclusion \(RFI\)  PHP Code Injection  Java Code Injection | HTTPoxy  Shellshock  Unix/Windows Shell Injection  Session Fixation  Scripting/Scanner/Bot Detection  Metadata/Error Leakages |
| :--- | :--- |


## Threat Intelligence

### OSINT

#### Search Engines

#### [Webmii - People search engine](https://webmii.com/)

#### [SearX - Privacy-respecting, hackable metasearch engine](https://github.com/searx/searx)

#### [Socialbearing - Twitter Analytics & Timelines](https://socialbearing.com/)

#### Reverse Image Search

```text
https://www.google.com/searchbyimage?image_url=
```

#### [OWASP Maryam](https://github.com/saeeddhqan/Maryam)

Open-source intelligence\(OSINT\) and Web-based Footprinting optional/modular framework based on the Recon-ng core and written in Python.

#### [OWASP Honeypot-Project](https://github.com/OWASP/Honeypot-Project)

The goal of the OWASP Honeypot Project is to identify emerging attacks against web applications and report them to the community, in order to facilitate protection against such targeted attacks.

#### [Docker MISP Container](https://github.com/harvard-itsecurity/docker-misp)

## Hardening / Standards

### [ASVS](https://github.com/OWASP/ASVS/tree/master) [Application Security Verification Standard](https://github.com/OWASP/ASVS/tree/master)

The standard provides a basis for designing, building, and testing technical application security controls, including architectural concerns, secure development lifecycle, threat modelling, agile security including continuous integration / deployment, serverless, and configuration concerns.

### [OWASP Mobile Application Security Verification Standard \(MASVS\)](https://github.com/OWASP/owasp-masvs)

The MASVS establishes baseline security requirements for mobile apps that are useful in many scenarios, including:

* In the SDLC - to establish security requirements to be followed by solution architects and developers;
* In mobile app penetration tests - to ensure completeness and consistency in mobile app penetration tests;
* In procurement - as a measuring stick for mobile app security, e.g. in form of questionnaire for vendors;
* Etc.

### [Software Assurance Maturity Model](https://owaspsamm.org/)

OWASP SAMM \(Software Assurance Maturity Model\) is the OWASP framework to help organizations assess, formulate, and implement a strategy for software security, that can be integrated into their existing Software Development Lifecycle \(SDLC\). OWASP SAMM is fit for most contexts, whether your organization is mainly developing, outsourcing, or acquiring software, or whether you are using a waterfall, an agile or devops method, the same model can be applied. This quick start guide walks you through the core steps to execute your OWASP SAMM-based secure software practice.

### [Docker Top 10](https://github.com/OWASP/Docker-Security)

Describes the most important 10 security bullet points for building a secure containerized environment. You can use it as a specification sheet if you start from scratch, alternatively handing it to a contractor who will do this for you.

### [Prowler](https://github.com/toniblyx/prowler)

Command line tool for AWS Security Best Practices Assessment, Auditing, Hardening and Forensics Readiness Tool.

## Knowledge

### OWASP

| Project Name | Description |
| :--- | :--- |
| [Security Knowledge Framework](https://github.com/blabla1337/skf-flask) | Our experience taught us that the current level of security of web-applications is not sufficient enough to ensure security. This is mainly because web-developers simply aren't aware of the risks and dangers that are lurking, waiting to be exploited by hackers. |
| [Threat Model Cookbook Index](https://github.com/OWASP/threat-model-cookbook/blob/master/INDEX.md) |  This project is about creating and publishing threat model examples into our [GitHub repository](https://github.com/OWASP/threat-model-cookbook). |
| [Security Shepherd](https://github.com/OWASP/SecurityShepherd) | The OWASP Security Shepherd Project is a web and mobile application security training platform. Security Shepherd has been designed to foster and improve security awareness among a varied skill-set demographic. The aim of this project is to take AppSec novices or experienced engineers and sharpen their penetration testing skill set to security expert status. |
| [XSS Filter Evasion Cheat Sheet](https://owasp.org/www-community/xss-filter-evasion-cheatsheet) | This article is focused on providing application security testing professionals with a guide to assist in Cross Site Scripting testing. |
| [Community Pages](https://owasp.org/www-community/) | OWASP Community Pages are a place where OWASP can accept community contributions for security-related content. To contribute, go to the [repository for this site](https://github.com/OWASP/www-community). Go into the `pages` folder and create a new file. Save and commit the file. |

### [SQL Injection Knowledge Base](https://www.websec.ca/kb/sql_injection)

### [Regular Expression Library](https://regexlib.com/)

