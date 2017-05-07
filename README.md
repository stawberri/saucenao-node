# saucenao-node
saucenao api wrapper

[![NPM](https://nodei.co/npm/saucenao.png?mini=true)](https://nodei.co/npm/saucenao/)

This package is an api wrapper for SauceNAO.

```js
const SauceNAO = require('saucenao')

let sauce = (await SauceNAO('picture.png')).json
```

## SauceNAO(input[, options])
* `input` *string or object.* An image url, filename, or `fs` input stream.
* `options` *object.* Any extra parameters you would like to pass in the request body.
* **Returns** *Promise.*
  * **Resolves** *Response.* A Node.js `IncomingMessage` with extra properties.
    * `body` *string.* The plain text body received from SauceNAO's servers.
    * `json` *object.* The body parsed as JSON (for your convenience).
  * **Rejects** *Error.* If anything goes wrong, you'll get an error.
    * `response` *Response.* If the error happened after the response was received (most likely because of a JSON parse error), the `IncomingMessage` that the promise resolves to will still be available here.  
    It is likely that `response.json` will not have been defined yet, but you may be able to print out `response.body` to figure out what went wrong.

This function is fairly simple.

If you provide a filename or a `fs` input stream as an input, it'll read your file for you so that you don't have to. If you instead pass it a url, it'll set the `url` parameter with it.

Then it'll send a request for you and attempt to parse the JSON in SauceNAO's response. It'll give you a promise that resolves to the HTTP response, with these values available as properties. If anything goes wrong, it'll reject an error with a `response` property so you can debug.


## new SauceNAO(api_key)
If you have [an api key](https://saucenao.com/user.php?page=search-api), you can specify it by passing it to SauceNAO's "constructor." This gives you a new function that works exactly the same way as the function above, but your requests will be authenticated.

```js
const SauceNAO = require('saucenao')
let mysauce = new SauceNAO('api_key here')

mysauce('picture.png').then((response) => {
  console.log('Request successful:')
  console.dir(response.json)
}, (error) => {
  console.log('Request encountered an error:')
  console.dir(error.request)
})
```
