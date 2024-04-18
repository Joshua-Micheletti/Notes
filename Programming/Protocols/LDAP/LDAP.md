LDAP is a communication protocol. It stands for **L**ightweight **D**irectory **A**ccess **P**rotocol.
Its definition is specified in the RFC4511.

Interaction with this protocol is kind of an inbetween HTTP and a Database:
You interact with it through LDAP calls and you access the content of a directory which is structured like a tree.

Communication through this protocol must be done by encoding the strings into Unicode (UTF-8).