What is an API (Application Programming Interface)?
API is like a waiter between your app and another system.
It lets two systems talk to each other.
Example: A weather app using an API to get weather data from a server.
-------
What is a REST API?
REST stands for Representational State Transfer.
REST API is a type of API that uses HTTP methods to perform actions.

Common HTTP Methods:
Method	Meaning	Action
GET	Get data	Read only (e.g., view data)
POST	Send new data	Create something new
PUT	Update existing data	Change/update something
DELETE	Delete data	Remove something
-------
What is an Endpoint?
An endpoint is simply a URL that your system (API/server) exposes to the outside world to perform some action or get data.
Think of it like a door to a room (functionality) in your server.
Example:
If your Nexus server is at http://192.168.1.10:8081,
an endpoint to list repositories might be:
http://192.168.1.10:8081/service/rest/v1/repositories
⏹That is an API endpoint where you can send a request using curl or wget to get info about repositories.
-------
How to Protect REST APIs?
Authentication: Require user/password or token (Nexus uses Basic Auth).
Authorization: Only allow users with proper roles to access certain endpoints (Nexus roles).
HTTPS: Use secure communication (SSL/TLS).
Rate Limiting: Prevent abuse by limiting requests.
API Keys: Give limited access via tokens.
JWT Tokens: Secure authentication with tokens (modern apps).
----
Curl Command Breakdown (API Usage):
curl -X GET -u admin:sayed@123 http://localhost:8081/service/rest/v1/repositories
curl: Command-line tool to send requests.
-X GET: Use GET method (can also use POST, PUT, DELETE, etc.).
-u admin:password: Username and password for Basic Auth.
URL: The endpoint you are calling.
----

