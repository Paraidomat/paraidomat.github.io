# DevNet Associate

## Software Development and Design

### Compare data formats (XML, JSON, YAML)

#### XML

[Source](https://www.w3schools.com/xml/xml_whatis.asp)

- Stands for eXtensible Markup Language
- is a markup language much like HTML
- was designed to store and transport data
- was designed to be self-descriptive
- is a W3C recommendation since 1998

Example:

```xml
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

#### JSON

- **J**ava **S**cript **O**bject **N**otation
- is a syntax for storing and exchanging data
- lightweight
- language independent

Example:

```json
{
  "name": "John", 
  "age": 30, 
  "city": "New York",
  "list": [
    "a", "list", "of", "strings"
    ],
  "this": {
    "even": "can",
    "be": "nested"
  }
}
```

#### YAML

[Source](https://yaml.org/spec/1.2/spec.html#Introduction)

- `YAML` = "YAML Ain't Markup Language"
- is a data serialization language
- designed to be human-friendly
- works well with modern programming languages
- YAML Goals:
  - easy readable by humans
  - data is portable between programming languages
  - matches the native data structures of agile languages
  - has a consistent model to support generic tools
  - supports one-pass processing
  - expressive and extensible
  - easy to implement and use
  
##### YAML Collections

- Use indentation for scope
- each entry begins on its own line
- Block sequences indicate each entry with a dash and a space (`- `)
- Mappings use colon and space (`: `) to mark each key value pair
- Comments begin with a `#`

- Examples:
  - Sequence of scalars
    ```yaml
    - foo bar
    - blupp bla
    - honky tonk
    ```
  - Mapping of scalars to scalars
    ```yaml
    foo: bar
    blupp: bla
    honky: tonk
    ```
  - Mapping Scalars to Sequences
    ```yaml
    nerd:
      - foo bar
    nonsense:
      - blupp bla
    instrument:
      - honky tonk
    ```

### Describe parsing of common data format (XML, JSON, and YAML) to Python data structures

#### XML

- [See this](https://docs.python.org/3/library/xml.html)

#### JSON

- To dump and load dictionaries to JSON use the [JSON](https://docs.python.org/3/library/json.html) module
  - `json.dump(obj, fp)` to serialize a phyton object to a file pointer
  - `json.dumps(obj)` to serialize to a JSON formated string.
  - `json.load(fb)` to deserialize the contents of a file, referenced by a file pointer to a python object
  - `json.loads(str)` to deserialize a string to a python object
  
#### YAML

- See [PyYAML](https://pyyaml.org/wiki/PyYAMLDocumentation)

### Describe the concepts of test-driven development

[Source](https://en.wikipedia.org/wiki/Test-driven_development)

![Test driven development](https://upload.wikimedia.org/wikipedia/commons/0/0b/TDD_Global_Lifecycle.png)

- Development-Cycle:
  1. Add a Test
    - each feature begins with writing a test.
    - Write a test that defines a function or improvements of a function, which should be very succinct.
  2. Run all tests and see if the new test fails
    - The validates, that the tests work correctly.
    - The test should fail for the expected reason.
    - This increases the confidence in the test.
  3. Write the code
    - Write code that causes the test to pass.
    - The new code may not be perfect, this is not an issue, due to Step 5.
  4. Run tests
    - If all test cases now pass, the programmer can be confident that the new code meets the test requirements and doesn't break anything else
  5. Refactor code
    - The growing code base must be cleaned up regularly during TDD
  6. goto 1.

### Compare software development methods (agile, lean, and waterfall)

#### Agile

- Agile software development values
  - __Individuals and interactions__ over processes and tools
  - __Working software__ over comprehensive documentation
  - __Customer collaboration__ over contract negotiation
  - __Responding to change__ over following a plan.
  
- Agile software development principles
1. Customer satisfaction by early and continuous delivery of valuable software.
1. Welcome changing requirements, even in late development.
1. Deliver working software frequently (weeks rather than months)
1. Close, daily cooperation between business people and developers
1. Projects are built around motivated individuals, who should be trusted
1. Face-to-face conversation is the best form of communication (co-location)
1. Working software is the primary measure of progress
1. Sustainable development, able to maintain a constant pace
1. Continuous attention to technical excellence and good design
1. Simplicity - the art of maximizing the amount of work not done — is essential
1. Best architectures, requirements, and designs emerge from self-organizing teams
1. Regularly, the team reflects on how to become more effective, and adjusts accordingly

#### Lean software development

[Source](https://en.wikipedia.org/wiki/Lean_software_development)

Can be summarized by seven principles, very close in concept to lean manufacutring principles:

1. Eliminate waste
1. Amplify learning
1. Decide as late as possible
1. Deliver as fast as possible
1. Empower the team
1. Build integrity in
1. Optimize the whole

#### Waterfall software development

### Explain the benefits of organizing code into methods / functions, classes, and modules

- Modularity for easier troubleshooting
- Reuse of code through inheritance
- Flexibility trough polymorphism
- Effective problem solving

### Identify the advantages of common design patterns (MVC and Observer)

#### Model View Controller

- Simultaneous development
  - The MVC decouples the various components of an application, developers are able to work in parallel on different components without affecting or blocking one another.
- High cohesion
  - MVC enables logical grouping of related actions on a controller together. 
  - The views for a specific model are also grouped together.
- Loose coupling
  - low coupling among models, views or controllers
- Ease of modification
  - Because of the separation of responsibilities, future development or modification is easier.
- Multiple views for a model.
- Testability

#### Observer

> The observer pattern is a software design pattern in which an object, called the **subject**, maintains a list of its dependents, called **observers**, and notifies them automatically of any state changes, usually by calling one of their methods.

### Explain the advantages of version control

- Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.
- Allows to compare files, identify differences, and merge changed if needed 

### Utilize common version control operations with Git

- Clone
  - Clones a repository into a newly created directory, creates remote-tracking branches for each branch in the cloned repository, and creates and checks out an initial branch that is fored from the cloned repositoy's currently active branch.
- Add/Remove
  - *Add*: This command updates the index using the current content found in the working tree, to prepare the content stanged for the next commit.
- Commit
  - Create a new commit containing the current contents of the index and the given log message describing the changes.
- Push/Pull
  - *Pull*: Incorporates changes from a remote repository into the current branch. `git pull` is a shorthand for `git fetch` followed by `git merge FETCH_HEAD`
  - *Push*: Updates remote refs using local refs, while sending objects necessary to complete given refs.
- Branch
  - List, create, or delete branches
  - If `--list` is given, or if there are no non-option arguments, existing branches are listed; the current branch will be highlighted in green and marked with an astersik.
  - Any branches checked out in linked worktrees will be highlighted in cyan and marked with a plus sign.
  - Option `-r` causes the remote-tracking branches to be listed, and option `-a` shows both local and remote branches.
- Merge and handling conflicts
  - Incorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch.
  - This command is used by `git pull` to incorporate changes from another repository and can be used by hand to merge changes from one branch into another.
- diff
  - Show changes between the working tree and the index or a tree, changes between the index and a tree, changes between two trees, changes resulting from a merge, changes between two blob objects, or changes between two files on disk. 

## Understanding and Using APIs

### Construct a REST API request to accomplish a task given API documentation

### Describe common usage patterns related to webhooks

> A webhook in web development is a method of augmenting or altering the behavior of a web page or web application with custom callbacks. 
> These callbacks may be maintained, modified, and managed by third-party users and developers who may not necessarily be affiliated with the orgininating website or application.
> The term is derived from the programming term [hook](https://en.wikipedia.org/wiki/Hooking).

- Format is usually JSON.
- Request is done as HTTP POST request.
- Webhooks are "user-defined HTTP callbacks".
- They are usually triggered by some event
  - When the event occurs, the source site makes an HTTP request to the URL configured for the webhook
  - Users can configure them to case events on one site to invoke behavior on another.

Use cases:
- CVS
- CI/CD systems
- Bug tracking systems

### Identify the constraints when consuming APIs

### Explain common HTTP response codes associated with REST APIs

TL;DR:

| Code | Meaning           |
| ---- | ----------------- |
| 2xx  | All good          |
| 4xx  | Client f\*cked up |
| 5xx  | Server f\*cked up |

For a complete list of HTTP Status codes see [developer.mozilla.org](https://developer.mozilla.org/de/docs/Web/HTTP/Status)

### Troubleshoot a problem given the HTTP response code, request and API documentation

### Identify the parts of an HTTP response (response code, headers, body)

### Utilize common API authentication mechanisms: basic, custom token, and API keys

### Compare common API styles (REST, RPC, synchronous, and asynchronous)

### Construct a Python script that calls a REST API using the requests library

[requests library documentation](https://requests.readthedocs.io/en/master/)

## Cisco Plaforms and Development

### Construct a Python script that uses a Cisco SDK given SDK documentation

### Describe the capabilities of Cisco network management platforms and APIs (Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, and NSO)

### Describe the capabilities of Cisco compute management platforms and APIs (UCS Manager, UCS Director, and Intersight)

### Describe the capabilities of Cisco collaboration platforms and APIs (Webex Teams, Webex devices, Cisco Unified Communication Manager including AXL and UDS interfaces, and Finesse)

### Describe the capabilities of Cisco security platforms and APIs (Firepower, Umbrella, AMP, ISE, and ThreatGrid)

### Describe the device level APIs and dynamic interfaces for IOS XE and NX-OS

### Identify the appropriate DevNet resource for a given scenario (Sandbox, Code Exchange, support, forums, Learning Labs, and API documentation)

### Apply concepts of model driven programmability (YANG, RESTCONF, and NETCONF) in a Cisco environment

### Construct code to perform a specific operation based on a set of requirements and given API reference documentation such as these:

- Obtain a list of network devices by using Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, or NSO
- Manage spaces, participants, and messages in Webex Teams
- Obtain a list of clients / hosts seen on a network using Meraki or Cisco DNA Center


## Application Deployment and Security

### Describe benefits of edge computing

### Identify attributes of different application deployment models (private cloud, public cloud, hybrid cloud, and edge)

### Identify the attributes of these application deployment types

- Virtual machines
- Bare metal
- Containers

### Describe components for a CI/CD pipeline in application deployments

### Construct a Python unit test

[unittest documentation](https://docs.python.org/3/library/unittest.html)

### Interpret contents of a Dockerfile

### Utilize Docker images in local developer environment

### Identify application security issues related to secret protection, encryption (storage and transport), and data handling

### Explain how firewall, DNS, load balancers, and reverse proxy in application deployment

### Describe top OWASP threats (such as XSS, SQL injections, and CSRF)

### Utilize Bash commands (file management, directory navigation, and environmental variables)

### Identify the principles of DevOps practices

## Infrastructure and Automation

### Describe the value of model driven programmability for infrastructure automation

### Compare controller-level to device-level management

### Describe the use and roles of network simulation and test tools (such as VIRL and pyATS)

### Describe the components and benefits of CI/CD pipeline in infrastructure automation

### Describe principles of infrastructure as code

### Describe the capabilities of automation tools such as Ansible, Puppet, Chef, and Cisco NSO

### Identify the workflow being automated by a Python script that uses Cisco APIs including ACI, Meraki, Cisco DNA Center, or RESTCONF

### Identify the workflow being automated by an Ansible playbook (management packages, user management related to services, basic service configuration, and start/stop)

### Identify the workflow being automated by a bash script (such as file management, app install, user management, directory navigation)

### Interpret the results of a RESTCONF or NETCONF query

### Interpret basic YANG models

### Interpret a unified diff

Let test1.py be:
```python
def test1(None):
    """ This is a file to test stuff """

    for i in range(0,10):
        print("this is awesome!")

    return 0
```

Let test2.py be:
```python
def test2(None):
    """ This is a file to test stuff """

    for i in range(0,10):
        print("this is Awesome!")
        print("and really cool!")

    return 0
```

Then the output of `diff -u test1.py test2.py` is:
```diff
paraidomat@zotac:~/difftest$ diff -u test1.py test2.py 
--- test1.py2020-09-30 10:41:01.427051114 +0200
+++ test2.py2020-09-30 10:42:09.691408548 +0200
@@ -1,7 +1,8 @@
-def test1(None):
+def test2(None):
     """ This is a file to test stuff """
 
     for i in range(0,10):
-        print("this is awesome!")
+        print("this is Awesome!")
+        print("and really cool!")
 
     return 0
```

> Note: Syntax highlighting is a feature of GitHub Flavored Markdown. 
> Usually the output ist just black and white.

### Describe the principles and benefits of a code review process

### Interpret sequence diagram that includes API calls

## Network Fundamentals

* Describe the purpose and usage of MAC addresses and VLANs
* Describe the purpose and usage of IP addresses, routes, subnet mask / prefix, and gateways
* Describe the function of common networking components (such as switches, routers, firewalls, and load balancers)
* Interpret a basic network topology diagram with elements such as switches, routers, firewalls, load balancers, and port values
* Describe the function of management, data, and control planes in a network device
* Describe the functionality of these IP Services: DHCP, DNS, NAT, SNMP, NTP
* Recognize common protocol port values (such as, SSH, Telnet, HTTP, HTTPS, and NETCONF)
* Identify cause of application connectivity issues (NAT problem, Transport Port blocked, proxy, and VPN)
* Explain the impacts of network constraints on applications

[For all of the above - see this](http://www.tcpipguide.com/free/t_NetworkingFundamentals.htm)
