# Asynchronous Tasks

If an API operation is asynchronous, but a client could track its progress, the response to such an asynchronous operation **MUST** return, in the case of success, the **202 Accepted** status code together with an `application/hal+json` representation of a new **task-tracking resource**.

## Task Tracking Resource

The task-tracking resource **SHOULD** convey the information about the status of an asynchronous task.

Retrieval of such a resource using the HTTP GET Request Method **SHOULD** be designed as follows:

1. Task is Still Processing

   Return **200 OK** and representation of the current status.

2. Task Successfully Completed

   Return **303 See Other** together with [HTTP Location Header](https://tools.ietf.org/html/rfc7231#section-7.1.2) with URI or a outcome resource.

3. Task Failed

   Return **200 OK** and `application/problem+json` with the problem detail information on the task has failed.

## Design Note

The asynchronous operation task-tracking resource can be either **polled** by client or the client might initially provide a **callback** to be executed when the operation finishes.

In the case of callback, the API and its client MUST agree on what HTTP method and request format is used for the callback invitation. 

### Example

1. **Initiate the asynchronous task**

   ```text
    POST /feeds/tasks/ HTTP/1.1
    Content-Type: application/json
    ...

    HTTP/1.1 202 Accepted
    Content-Type: application/hal+json
    Retry-After: 60

    {
      "_links": {
        "self": { "href": "/feeds/tasks/1" }
      },
      "message": "Your task to generate feed has been accepted. Try query for result after 60 seconds.",
      "retryAfter": 60
    }
   ```

2. **Poll the task status: In progress**

   ```text
    GET /feeds/tasks/1 HTTP/1.1
    ...

    HTTP/1.1 200 Ok
    Content-Type: application/hal+json
    Retry-After: 30

    {
      "_links": {
        "self": { "href": "/feeds/tasks/1" }
      },
      "message": "Your feed is being generated. Try query for result after 30 seconds.",
      "retryAfter": 30
    }
   ```

3. **Poll the task status: Finished**

   ```text
    GET /feeds/tasks/1 HTTP/1.1
    ...

    HTTP/1.1 303 See Other
    Location: /feeds/1
    Content-Location: /feeds/tasks/1
    Content-Type: application/hal+json

    {
      "_links": {
        "self": { "href": "/feeds/tasks/1" },
        "feed": { "href": "/feeds/1" }
      },
      "message": "Your feed is ready."
    }
   ```

4. **Poll the task status: Failure**

   ```text
    GET /feeds/tasks/1 HTTP/1.1
    ...

    HTTP/1.1 200 OK
    Content-Type: application/problem+json

    {
      "title": "Wrong input parameters",
      "detail: "Missing required input parameter XYZ.",
      "status": 400
    }
   ```

