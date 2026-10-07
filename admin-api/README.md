# Introduction

## General

Exberry Admin API allows to manage the exchange static data (instruments, trading calendars and more) as well as sending operational commands (EOD, halt trading and more).

Sandbox environment endpoint (`URL_ORIGIN`): `https://admin-api.uat.exberry-uat.io`

Guidelines:

* All numbers are stringified unless explicitly mentioned otherwise
* Numeric values (integers and decimals) may be sent either as numbers or as strings. Responses will always return them as strings
* Optional fields should be omitted from request if not required
* System ignores any additional parameter that are sent on request body but was not specified in this document

### **Maker-Checker**&#x20;

**Maker-Checker approval flow** (to activate, contact the operations team):

The maker-checker process is a workflow that requires the approval of at least two individuals to agree on an action. It involves two parties:

* **Maker**: the person who initiates the action (can always see their own requests and reject them)
* **Checker**: the person who approves the action (cannot approve their own request)

When a user requests an action (like update or create an instrument), the change does not happen immediately - e.g. it won't be shown in the instrument list. The change takes place only after approval. A super user with the right permission can override this flow.

For any action that is part of the flow, the request can include a `comment` field (String) - the maker's explanation of what the action contains, giving the checker the change context.

Instead of the regular response message, any API request involved in the flow returns a `MakerCheckerRequest` entity (see the model below). Its `data` property contains the data model of the requested action, identical to the API specification for that action without maker-checker activated.

***

### **Error Handling**

Error response contains the following fields:

| Name    | Description   |
| ------- | ------------- |
| code    | Error code    |
| message | Error message |

\
Error response sample:

```json
{
    "code": 10001,
    "message": "Permission denied"
}
```

In case of wrong path, generic error will be returned:

```json
{
    "message": "Route not found",
    "code": 1
}
```

**Generic Error Codes**

<table><thead><tr><th width="230">Code</th><th></th></tr></thead><tbody><tr><td>1</td><td>One of the below:<br>- Timeout expired<br>- Exchange is unavailable<br>- Invalid JSON<br>- Route not found<br>- Maximum request size is 60kb</td></tr><tr><td>10000</td><td>Invalid token</td></tr></tbody></table>

#### HTTP Error codes

400: When exceeding Maximum request size of 60KB or Invalid JSON\
403: Authentication and authorization related errors\
404: Route not found\
401: Invalid token error\
503: System is not available\
504: Timeout\
500: Any other error
