Project Description: HTTP Server Implementation in Spring Boot

This project is a custom HTTP server built using Spring Boot, with a focus on handling various aspects of HTTP communication, including request parsing, response building, header validation, and error handling. The server processes incoming HTTP requests, supports HTTP/1.1 compatibility, and serves files or resources based on specific configurations. Key components include classes for HTTP request parsing, HTTP version compatibility, customizable HTTP responses, and robust error handling.

Key Components and Functionality
1. HttpParser
The HttpParser class handles parsing of the HTTP request. It processes the request line (method, target URI, HTTP version) and request headers, and it can also parse the message body if present.
Key methods:
parseHttpRequest: Initializes parsing of the request line, headers, and body from an input stream.
parseRequestLine: Extracts the HTTP method, request target, and HTTP version.
parseHeaders: Parses and validates each header line, detecting the end of the headers section based on double CRLF.
processSingleHeaderField: Uses regex to validate header format and extract the field name and value.
The parser also enforces constraints, such as method length and HTTP version validation, and raises HttpParsingException for invalid inputs.
2. HttpRequest
Extends HttpMessage and represents an HTTP request, encapsulating information about the HTTP method, request target, HTTP version, and headers.
Supports methods to set and retrieve the request’s HTTP method (GET, HEAD), target URI, and best compatible HTTP version.
The class enforces HTTP compliance, throwing exceptions if the method is unsupported or the URI is malformed.
3. HttpResponse
Extends HttpMessage and represents an HTTP response, including the status line (HTTP version, status code, reason phrase), headers, and optional body.
Builder Pattern: The HttpResponse class includes a nested Builder class, which allows for a flexible and clear construction of responses. Using this builder pattern, users can set the HTTP version, status code, reason phrase, headers, and message body for the response.
getResponseBytes: This method constructs the byte array for the response, including headers and body, suitable for transmission over a socket.
4. HttpMessage (Abstract Class)
HttpMessage serves as a base class for HttpRequest and HttpResponse, encapsulating common functionality related to HTTP headers and message bodies.
Manages headers in a case-insensitive HashMap and provides methods for header manipulation (addHeader, getHeader) and body management (getMessageBody, setMessageBody).
5. HttpMethod (Enum)
Defines supported HTTP methods, such as GET and HEAD, with constraints like maximum method length to prevent unsupported methods.
The MAX_LENGTH constant calculates the maximum method name length dynamically, used in HttpParser to validate incoming methods.
6. HttpHeaderName (Enum)
Enumerates standard HTTP header names, currently including Content-Type and Content-Length, simplifying header management by providing standardized values.
7. HttpVersion (Enum)
Encodes support for HTTP/1.1, including version literals (HTTP/1.1), major and minor version numbers, and methods for version compatibility checks.
getBestCompatibleVersion: Uses a regex-based matcher to find the closest compatible version for incoming requests based on their HTTP version, raising BadHttpVersionException for unsupported versions.
8. HttpStatusCode (Enum)
Enumerates HTTP status codes with corresponding messages, including common client errors (e.g., 400 Bad Request, 404 Not Found) and server errors (e.g., 500 Internal Server Error, 505 HTTP Version Not Supported), as well as success codes (e.g., 200 OK).
The enum provides STATUS_CODE and MESSAGE fields, making it easy to reference specific HTTP statuses across the server.
9. HttpParsingException (Exception Class)
Custom exception to handle parsing errors in HttpParser, including client and server error codes.
Stores an HttpStatusCode representing the error code and provides methods to retrieve error information, enabling consistent error handling across the server.
10. BadHttpVersionException (Exception Class)
Custom exception thrown when an HTTP version string cannot be parsed or matched to a compatible version.
Used specifically in HTTP version validation within the HttpVersion enum to signal unsupported HTTP versions.
Additional Features
Logging: The HttpParser uses SLF4J logging to debug the processing of the request line and headers, allowing developers to track parsing details and diagnose issues.
Error Handling: The server provides robust error handling through custom exceptions and pre-defined HTTP status codes, ensuring that client and server errors are clearly communicated.
Response Customization: With the HttpResponse.Builder class, developers can easily customize responses based on specific requirements, making it adaptable for serving various file types or handling dynamic content.
HTTP Compliance: The project enforces HTTP standards by validating request method length, header formats, and HTTP version compatibility, ensuring that the server responds accurately to various client requests.
Example Workflow
Request Parsing: An incoming HTTP request stream is passed to HttpParser, which parses the request line, headers, and body, raising exceptions for malformed requests.
Header Validation: Headers are processed and validated, with support for headers like Content-Type and Content-Length.
Response Building: HttpResponse.Builder is used to construct a response, setting appropriate HTTP status, headers, and body based on the request outcome.
Response Transmission: The final response byte array, including status line, headers, and body, is transmitted to the client, following the HTTP/1.1 specification.
This project provides a foundational HTTP server with modular, customizable components that can be extended to serve files, manage sessions, and handle complex request-response flows. It combines core HTTP processing with Spring Boot’s framework capabilities, making it suitable for lightweight web services, embedded server applications, or educational purposes in learning HTTP protocol implementation.






