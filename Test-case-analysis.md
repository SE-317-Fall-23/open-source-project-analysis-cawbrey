# Assignment Submission

## Project Selected: Express

## I. Introduction
- Briefly introduce the purpose of this section, which is to provide a detailed description of two test cases in the selected open-source project.

## II. Test Case 1: utils.etag(body, encoding)
### A. Description
- These tests collectively assess the etag function's behavior with different input types, covering scenarios such as regular strings, UTF-8 strings, Buffers, and empty strings. The expected ETag values serve as benchmarks to verify the correctness of the function's output.
### C. Test Steps
1. Create an etag with given string
2. Check if the etag generated matches the expected etag.
### D. Code Segments Under Test
- This code snippet is from lib/utils.js and it converts any body given to into an etag, and allows options.

```JavaScript
exports.etag = createETagGenerator({ weak: false })


function createETagGenerator (options) {
  return function generateETag (body, encoding) {
    var buf = !Buffer.isBuffer(body)
      ? Buffer.from(body, encoding)
      : body

    return etag(buf, options)
  }
}

```
### E. Initial State
- This is utility class and doesn't depend on the other aspects of the system in order to operate, it's very basic in this way.
### F. Transition
- This test doesn't really change anything with the overall system because all it is doing is testing a simple utility function. You could use these eTags in the system for other things, but this doesn't inherently change anything at all.
### G. Expected State
- We expect to get an etag that has been generated from the given body, and we expect it to work if given almost any body, including strings, UTF-8 strings, buffers, and an empty string. 

## III. Test Case 2: res.get.js
### A. Description
- This test case is for the res.get(field) method in Express.js. It verifies that the res.get function retrieves the correct value for a specified response header field. The test sets up a minimal Express application with a middleware that sets the 'Content-Type' header to 'text/x-foo'. Then, it sends a request to the application and checks if the response contains the expected 'Content-Type' header value ('text/x-foo'). The test passes if the response matches the expected value and the status code is 200.
### C. Test Steps
1. Setup Express app
2. Sets headers response content of the test cases
3. Request the header from express
4. Check if the response has a 200 response code and the correct header information.
### D. Code Segments Under Test
- This is primarily trying to test the get method of express and see if it is able to return the correct header, body, and response code. These are set using the setHeader, and the send methods for the request and response classes.
```JavaScript
SendStream.prototype.setHeader = function setHeader (path, stat) {
  var res = this.res

  this.emit('headers', res, path, stat)

  if (this._acceptRanges && !res.getHeader('Accept-Ranges')) {
    debug('accept ranges')
    res.setHeader('Accept-Ranges', 'bytes')
  }

  if (this._cacheControl && !res.getHeader('Cache-Control')) {
    var cacheControl = 'public, max-age=' + Math.floor(this._maxage / 1000)

    if (this._immutable) {
      cacheControl += ', immutable'
    }

    debug('cache-control %s', cacheControl)
    res.setHeader('Cache-Control', cacheControl)
  }

  if (this._lastModified && !res.getHeader('Last-Modified')) {
    var modified = stat.mtime.toUTCString()
    debug('modified %s', modified)
    res.setHeader('Last-Modified', modified)
  }

  if (this._etag && !res.getHeader('ETag')) {
    var val = etag(stat)
    debug('etag %s', val)
    res.setHeader('ETag', val)
  }
}
```
### E. Initial State
- The initial state involves an instance of an Express application (app) being created, with a middleware setting the 'Content-Type' response header to 'text/x-foo', and an HTTP GET request yet to be made.

### F. Transition
- The transition occurs when an HTTP GET request is made to the root ('/') of the Express application using the request library, triggering the middleware to set the response header and send the header value as the response content.
### G. Expected State
- The expected state is a successful HTTP response (status code 200) with the response body containing the value 'text/x-foo', confirming that the res.get method correctly retrieves and returns the specified 'Content-Type' response header field.