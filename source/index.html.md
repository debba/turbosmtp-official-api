---
title: API Reference

toc_footers:
  - <a href='https://www.serversmtp.com'>Go to turboSMTP website</a>
  - <a href='https://www.serversmtp.com/event-webhook-reference/'>Go to turboSMTP webhooks reference</a>
  - <a href='https://dashboard.serversmtp.com'>Go to turboSMTP dashboard</a>
  
language_tabs:
- json: Request/Response details

includes:
  - webhooks
  - send_mail
  - accounts
  - reports
  - statistics
  - tools
  - errors

search: true
---

# turboSMTP Public API
  
## Version 1.0

This document contains information about turboSMTP's public API. Please report bugs to: <api@turbo-smtp.com>

# Getting started
  
## Data interchange format

For the most part (and where not otherwise explicit) turboSMTP’s API uses JSON as the data format of choice when it comes to request and response bodies.

## Accessing resources

Authorization to access a user’s resource is granted to clients provided they set a authentication cookie into their request, valued with the proper authentication key issued by turboSMTP servers. The authentication key is user-based and it is issued by turboSMTP servers upon successful user’s email address + password challenge, performed by means of appropriate request.

As an example, such cookie or request header should look like what follows:

`Authorization: 44cf4c36d0e9cbe32f6fd83ff69a9df3b6212828c`

## Authentication (a.k.a. login)

> Request headers:

```
Content-Type: application/json; charset=utf-8
```

> Request Body Example

```json

{
	"email": "turboSMTP email address",
	"password": "turboSMTP password",
    "no_expire": "1"
}
```

> Response headers:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

> Response body (successful authentication):

```json
{
    "auth" : "44cf4c36d0e9cbe32f6fd83ff69a9df3b62128s28c"
}
```

> Response body (unsuccessful authentication):

```json
{
    "errorCode" : 1,
    "message": "Wrong credentials specified"
}
```

The `auth` value in the response has to be used as a custom header or cookie for all requests towards turboSMTP's API.

### Request endpoint:

`
POST https://dashboard.serversmtp.com/api/authorize
`

- `email` is the email of turboSMTP account
- `password` is the password of turboSMTP account
- `no_expire` (optional) in this case session doesn't expire.

If no_expire value is not specified or is set to 0, the authentication will expire in 1 hour, otherwise it will not expire.

You can revoke API access using the **deauthorize** endpoint.

## Revoke API Access

> Request headers:

```
Content-Type: application/json; charset=utf-8
Authorization: "your authorization token"
```

> Response headers:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

> Response body (successful authentication):

```json
{
    "message": "Token deauthorized"
}
```

> Response body (unsuccessful authentication):

```json
{
    "errorCode": 8,
    "message": "No authorization key was specified for request"
}
```

```json
{
    "errorCode": 9,
    "message": "This authorization token doesn't exist."
}
```

### Request endpoint:

`
POST https://dashboard.serversmtp.com/api/deauthorize
`

No params needed
