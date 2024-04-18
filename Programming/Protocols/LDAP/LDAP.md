LDAP is a communication protocol. It stands for **L**ightweight **D**irectory **A**ccess **P**rotocol.
Its definition is specified in the RFC4511.

Interaction with this protocol is kind of an inbetween HTTP and a Database:
You interact with it through LDAP calls and you access the content of a directory which is structured like a tree.

Communication through this protocol must be done by encoding the strings into Unicode (UTF-8).

## Bind
This term in LDAP terminology represents an authentication. The client communicating to a server with LDAP protocol needs to perform a **Bind** to be authorized.

The Bind can happen in 3 ways:
- Anonymous Bind:
	- The client doesn't provide any credentials and is allowed inside the directory of the server.
	- Not secure and usually you shouldn't expose much of the directory to Anonymous users
- Simple password Bind:
	- The client needs to provide a valid password to be authenticated