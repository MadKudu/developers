# Errors

## Status codes

Our API returns standard HTTP codes.

For errors, we will also include extra information about what went wrong encoded in the response as JSON. The various HTTP status codes we might return are listed below:

Error Code | Title | Meaning
---------- | ----- | -------
400 | Bad Request | Make sure you entered all the required parameters
401 | Unauthorized | Your API key is invalid
404 | Not Found | The resource does not exist
50x | Internal server error | An error occurred on our end. Not your fault, we’re looking into it

> Example error response:

```json
{
  "message": "Authorization error: Invalid API Key"
}
```
