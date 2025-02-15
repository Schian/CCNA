# Access Control Lists

## Standard ACLs

- **Access Control Lists (ACLs)** function as a packet filter
  - Instructions for the router to permit or discard traffic
  - Standard ACLs filer only on source IP
    - Can be used to filter in or outbound traffic
    - Extended ACLs have more parameters they can filter on
- ACLs are configured globally on the router
  - As an ordered sequence of **Access Control Entries (ACEs)**
- Once configured an ACL is applied to an interface
  - Only one inbound and one outbound ACL can be applied to each interface
- The router checks the packet against the ACL
  - Processes the ACEs from top to bottom
  - If a match is found, no further ACEs are checked
- **Implicit Deny**
  - If a packet doesn't match any ACE, the packet is dropped
  - To avoid this, a `permit any` ACE is required

### Standard Numbered ACLs

- Numbered ACLs are identified with a number
  - ACL1, ACL 2, etc
- Different types of ACLs have a different range of numbers that can be used
  - Standard ACLS can used 1-99 and 1300-1999
    - This is the *Standard IP* range
- **Standard ACLS should be applied as close to the destination as possible**

### Standard Named ACLs

- Named ACLS are identified with a name
  - Don't use spaces in the name
- Configured from the *'Standard Named ACL Config Mode'*
  - Then configure each entry within this mode
- The router may re-order any /32 entries
  - This improves the efficiency of processing the ACL
  - This does not change the effect of the ACL

## Configuration

### Standard ACLs - Configuration

- Show the configured standard **numbered** ACLs
  - `R1(config)#show ip access-lists`
- Show the configured standard **named** ACLs
  - `R1#show access-lists`
- Configure a standard numbered ACL
  - `R1(config)#access-list <number> {deny | permit} <ip> <wildcard-mask>`
    - `R1(config)#access-list 1 deny 2.2.2.2 0.0.0.0`
    - `R1(config)#access-list 1 deny 2.2.2.2`
    - `R1(config)#access-list 1 deny host 2.2.2.2`
    - `R1(config)#access-list 1 permit any`
    - `R1(config)#access-list 1 permit 0.0.0.0 255.255.255.255`
- Configure a remark for a standard numbered ACL
  - `R1(config)#access-list <number> remark <string>`
- Configure a standard named ACL
  - `R1(config)#ip access-list standard <acl-name>`
  - `R1(config-std-nacl)#[entry number] {deny | permit} <ip> <wildcard-mask>`
- Apply ACL to an interface
  - `R1(config-if)#ip access-group {number | name} {in | out}`

### Extended ACLs - Configuration
