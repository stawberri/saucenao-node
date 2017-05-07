# saucenao-node
saucenao api wrapper

[![NPM](https://nodei.co/npm/saucenao.png?mini=true)](https://nodei.co/npm/saucenao/)

This package is an api wrapper for SauceNAO.

```js
const SauceNAO = require('saucenao')

let sauce = (await SauceNAO('picture.png')).json
```

## SauceNAO(input[, options])
* `input` *string or object.* An image url, filename, `Buffer`, or `fs` readable stream.
* `options` *object.* Any extra parameters you would like to pass in the request body.
* **Returns** *Promise.*
  * **Resolves** *Response.* A Node.js `IncomingMessage` with extra properties.
    * `body` *string.* The plain text body received from SauceNAO's servers.
    * `json` *object.* The body parsed as JSON (for your convenience).
  * **Rejects** *Error.* If anything goes wrong, you'll get an error.
    * `response` *Response.* If the error happened after the response was received (most likely because of a JSON parse error), the `IncomingMessage` that the promise resolves to will still be available here.  
    It is likely that `response.json` will not have been defined yet, but you may be able to print out `response.body` to figure out what went wrong.

This function is fairly simple. It mostly just performs a request to SauceNAO for you, so that you can skip a couple function calls. It can work with local filepaths, urls that begin with `http://` or `https://`, `fs` input streams, or even buffers.

After it receives a reply from SauceNAO, it'll attempt to parse it as JSON and add it to the Response object for your convenience. It'll return a promise that either resolves to this extended Response object or rejects an error containing as much of this extended Response object as it generated.


## new SauceNAO(api_key)
If you have [an api key](https://saucenao.com/user.php?page=search-api), you can specify it by using this function like a constructor. This gives you a new function that works exactly the same way as the function above, but your requests will be authenticated.

```js
const SauceNAO = require('saucenao')
let mySauce = new SauceNAO('api_key here')

mySauce(imageDataBuffer).then((response) => {
  console.log('Request successful')
  console.dir(response.json)
}, (error) => {
  console.error('Request encountered an error')
  console.dir(error.request)
})
```
