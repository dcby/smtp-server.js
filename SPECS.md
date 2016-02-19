# Node.js based SMTP re-transmitter
The software must be implemented in JavaScript using node.js runtime. At this moment the preferable supported version of node.js is 4.3.1. The software must fully adopt asynchronous nature of V8 engine to optimize receive/send speed. The software must seamlessly run on different platforms such as Linux and Windows.
### Main features
* Configure using YAML file(s).
* Listen on configured port.
* Receive email locally.
* Transmit email to designated remote recipient using standard SMTP pipeline (MX lookup, etc).
* In case of temporary delivery failure (4XX or related) the farther attempts must be deferred by configurable amount of time.
* In case of permanent delivery failure (5XX or related) or consecutive temporary failures email must be deleted from queue and report must be sent to sender using standard SMTP pipeline (MX lookup, etc) using null-from address `<>`.
* Select outgoing IP address and EHLO argument based on specific rules (see section below).
* Log activity.

### Select outgoing IP address
Sending part of software must be able to bound to specific local IP address and choose EHLO argument for outgoing connection. The main factors are FROM domain and incoming local IP. The configuration file may contain a map of domain, IP and hostname entries. Example...
```yaml
  - domain: test.com
    ip: 10.10.10.10
    hostname: mta.test.com
  - domain: test2.com
    ip: 10.10.10.11
    hostname: mail.test2.com
```
 The basic rules are:
* If domain - IP rule is found in the configuration and rule matches FROM domain then IP address and hostname from configuration are used.
* If no rule exists the outgoing IP address must be picked up randomly among available local IPs. The hostname must match IP address in configuration or default hostname used.
* The client must be able to override that rules while submitting emails. This could be done using private SMTP extension command or via specially shaped EHLO argument. This is a subject to discuss.

### User interface
The simple UI is required to modify configuration file(s) on Windows. The preferred technology for this is Windows Presentation Foundation (.Net, C#, XAML) although other solutions are also possible. The final configuration layout is not yet determined, but basically the UI must allow to manage next values:
* Default hostname of the server.
* Listen ports.
* Throttling and queue management.
* Logging settings.
* Domain/IP/hostname map.
