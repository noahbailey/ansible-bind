# ansible-bind
Ansible role for managing bind9 DNS servers

This role takes data from ansible variables and uses it to set up bind9 (debian flavour) DNS. 

Currently, this role only configures servers and zones. It does not yet handle replication in primary/secondary systems. 

## Usage

Add this role to the `roles` directory in your ansible project.

Then, include the role using a top level playbook:

```yaml
- name: DNS Servers
  hosts: dns-servers
  become: true 
  roles: 
  - ansible-bind
```



## Variables

This role requires these variables to exist in inventory:

#### 1.  bind (sever config)

This establishes the basic parameters of the server, such as where to send requests, and where to respond to them from. 

```
bind: 
  trusted_subnets: 
    - 10.11.12.0/24
  forwarders: 
    - 1.1.1.1
    - 1.0.0.1
```



#### 2. zones 

This defines the individual zones that the server will host. 

zones is structured as a list of maps, meaning that each one will be separately rendered into a bind database file. 

Note that the first NS record is automatically created using the `primary_ns` key, which is used to also construct the SOA record. 

Each record will be placed into the file exactly as it is in the list, so formatting is important. 

Example: 

```yaml
- name: onetwoseven.one
  primary_ns: dns1.onetwoseven.one
  records: 
    - '@        IN    A     10.204.11.20'
    - 'dns1     IN    A     10.204.10.11'
```

