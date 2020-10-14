# Errors

<<<<<<< HEAD
## Status codes
=======
<aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside>
>>>>>>> 6f24a0df7a5d5f2a0517a69728818a65d67c60aa

Our API returns standard HTTP codes.

For errors, we will also include extra information about what went wrong encoded in the response as JSON. The various HTTP status codes we might return are listed below:

<<<<<<< HEAD
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
=======
Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The kitten requested is hidden for administrators only.
404 | Not Found -- The specified kitten could not be found.
405 | Method Not Allowed -- You tried to access a kitten with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The kitten requested has been removed from our servers.
418 | I'm a teapot.
429 | Too Many Requests -- You're requesting too many kittens! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
>>>>>>> 6f24a0df7a5d5f2a0517a69728818a65d67c60aa
