The architecture is divided between **Client** and **Server**:
## Client
The client is the user application, it can be either **Mobile (Flutter)** or **Web (Angular)**.

Some of the web applications are:
- **portale-web**
- **dashboard**
- **portale-saas**

The clients connect to the **Web Server** to retrieve information to display and interact with.
## Server
This is where the Client connects to in order to retrieve information and functionalities.

The first point of contact of the Clients is the **Web Server**

### Web Server
The Web Server is an HTTP server that contains and redirects HTTP requests to the correct locations.

This is done locally with an HTTP Apache Server, i don't know yet how it's composed on deploy.

The Web Applications live and are accessed from the Web Server, pointing at the correct directions (**routes**), defined inside the configuration of the web server itself.

The Web Apps:
- portale-web
- dashboard
- portale-saas
Access the underlying structure and services through an **OAuth** server

### 


