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
- **Standard ACLS should be applied as close to the `destination` as possible**

### Standard Named ACLs

- Named ACLS are identified with a name
  - Don't use spaces in the name
- Configured from the *'Standard Named ACL Config Mode'*
  - Then configure each entry within this mode
- The router may re-order any /32 entries
  - This improves the efficiency of processing the ACL
  - This does not change the effect of the ACL

#### Named ACL Config Mode

- Numbered ACLs can also be configured from *Named ACL Config Mode*
  - Individual entries can be deleted in this mode
    - `no [entry number]`
    - Trying to delete an individual entry in *global config mode* will delete the entire ACL
  - An ACE can be placed between two other ACEs by specifying the sequence number
  - ACLs can be resequenced from this mode to help keep the ACL clear and can help with future editing
    - `ip access-list resequence <acl-id> <starting-seq-num> <increment>`

## Extended ACLs

- The range for Extended ACLs is: **100-199, 2000-2699**
- Can be matched on more parameters making them much more precise
  - Also more complex
- Can be match on port numbers using the following keywords:
  - `eq` - Equal to port ...
  - `gt` - Greater than port ...
  - `lt` - Less than port ...
  - `neq` - Not equal to port ...
  - `range <from> <to>` - From port ... to port ...
- If any extra conditions are specified, they must all match
  - Close enough isn't good enough
  - It either matches or it doesn't
- **Extended ACLs should be applied as close to the `source` as possible**

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

- Delete an ACE from an ACL
  - `R1(config)#ip access-list standard 1`
  - `R1(config-std-nacl)#no [entry number]`
- Insert a new ACE in between two other entries
  - `R1(config-std-nacl)#[specific sequence number] {deny | permit} <ip> <wildcard-mask>`
- Resequence the ACL
  - `R1(config)#ip access-list resequence <acl-id> <starting-seq-num> <increment>`
    - `ip access-list resequence 1 5 10`
    - Resequence ACL 1 with the first entry starting at 5, incrementing by 10 for each following entry
- Configure a numbered extended ACL
  - `R1(config)#access-list <number> {permit | deny} <protocol> <src-ip> <dest-ip>`
- Configure an extended ACL from the ACL config mode
  - `R1(config)#access-list extended {name | number}`
  - `R1(config-ext-nacl)#[seq-num] {permit | deny} <protocol> <source-ip> [pt-match] [source pt] <dest-ip> [pt-match] [dest-port]`
