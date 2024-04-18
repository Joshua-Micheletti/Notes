LDAP is a communication protocol. It stands for **L**ightweight **D**irectory **A**ccess **P**rotocol.
Its definition is specified in the RFC4511.

Interaction with this protocol is kind of an inbetween HTTP and a Database:
You interact with it through LDAP calls and you access the content of a directory which is structured like a tree.
The tree like structure of data inside a LDAP server is called the: **DIT** (**D**irectory **I**nformation **T**ree).

Communication through this protocol must be done by encoding the strings into Unicode (UTF-8).

## Bind
This term in LDAP terminology represents an authentication. The client communicating to a server with LDAP protocol needs to perform a **Bind** to be authorized.

The Bind can happen in 3 ways:
- Anonymous Bind:
	- The client doesn't provide any credentials and is allowed inside the directory of the server.
	- Not secure and usually you shouldn't expose much of the directory to Anonymous users
- Simple password Bind:
	- The client needs to provide a valid password and a username (**DN**, **D**istinguished **N**ame) to be authenticated
	- Equivalent to HTTP authorization
	- Also known as **cleartext**, these connections happen through protocol: **ldap://**
- SASL:
	- **S**imple **A**uthentication and **S**ecurity **L**ayer
	- Uses cripting algorithms to encode the credentials
	- Equivalent to HTTPS authorization
	- These connections happen through protocol: **ldaps://**

## LDIF
**L**DAP **D**ata **I**nterchange **F**ormat represents the extensions of the files used to add or modify entries inside a LDAP server.

## Server information
With the LDAP protocol it's possible to make requests to the server asking for its information and its **schema** (how the data is stored in it and what types of data it understands).

The server will also tell you which **Controls** and **Extensions** it supports:
### Controls
They are modifications to the standard LDAP operations
### Extensions
They are new operations developed on top of the LDAP standard 
## Distinguished Name (DN)
The Distinguished Name (DN) represents a path in the tree structure of the LDAP server. It's a string of key-value pairs that represents the path to take from a root of the tree to find the specified object. The pairs are separated by a comma and its order represents its height in the tree, with the first entry representing the leaf and the last entry representing the root.

Each DN is **unique** in the DIT, as it represents a path. It will change only if the internal structure of the DIT is changed.

The parts that make up the DN (separated by commas and each representing a path of a layer in the tree) are called **RDN** (**R**elative **D**istinguished **N**ame).

For example, this DN identifies a single entry in the DIT:
"cn=Fred,ou=users,o=company"

It's composed of 3 RDNs:
- "cn=Fred" is the leaf RDN (**C**ommon **N**ame)
- "ou=users" is the middle branch RDN that represents the **O**rganizational **U**nit (a sort of group of leaves or other ou)
- "o=company" is the root RDN that represents the **O**rganization (often represent root nodes)