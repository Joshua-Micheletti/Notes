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
- SASL:
	- **S**imple **A**uthentication and **S**ecurity **L**ayer
	- Uses cripting algorithms to encode the credentials
	- Equivalent to HTTPS authorization

## LDIF
**L**DAP **D**ata **I**nterchange **F**ormat represents the extensions of the files used to add or modify entries inside a LDAP server.

## Server information
With the LDAP protocol it's possible to make requests to the server asking for its information and its **schema** (how the data is stored in it and what types of data it understands)

## Distinguished Name
The Distinguished Name (DN) represents a path in the tree structure of the LDAP server. It's a string of key-value pairs that represents the path to take from a root of the tree to find the specified object. The pairs are 