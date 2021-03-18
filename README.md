# APEX + MLE
An APEX app which demonstrates the power of server-side JavaScript via MLE &amp; GraalVM

## App

Download app.sql, and import it into an APEX 20.2 instance, on an Always Free 21c DB.

## Examples

| Name | Description | Libraries Used |
| --- | --- | --- |
| Form Validation | Use form validations such as isEmail or isCreditCard | validator |
| QR Code Generation | Generate a QR Code as a blob or base64 encode string | qrcode |
| Markdown to HTML | Convert Markdown to HTML serverside. Integration with dompurify would also be wise | marked |
| HTML Sanitization | Clean HTML of XSS vunrabilities and other issues | text-encoding, mle-jsdom, dompurify |
| Image Manipulation | Experimental example for cropping, flipping, and watermarking images | jimp |

## Libraries

To be able to run the examples, navigate to the Modules page and create extermal modules for the following libraries.
On save, the actual code will be downloaded automatically.

| Library name | URL |
| --- | --- |
| dompurify | https://cdnjs.cloudflare.com/ajax/libs/dompurify/2.2.7/purify.min.js |
| jimp | https://cdn.jsdelivr.net/npm/jimp@0.16.1/browser/lib/jimp.js |
| marked | https://cdnjs.cloudflare.com/ajax/libs/marked/1.2.9/marked.min.js |
| mle-jsdom | https://raw.githubusercontent.com/stefandobre/mle-jsdom/main/jsdom.js |
| qrcode | https://cdnjs.cloudflare.com/ajax/libs/qrcode-generator/1.4.4/qrcode.min.js |
| text-encoding | https://cdn.jsdelivr.net/npm/text-encoding@0.6.4/lib/encoding.min.js |
| validator | https://cdnjs.cloudflare.com/ajax/libs/validator/13.5.2/validator.min.js |

## Loading modules

You will notice an application process called `Setup MLE` that runs After Page Submission. This process exposes 2 global helper functions: `requireModule` and `loadScript`. Based on a module name, these functions will fetch the CLOB code out of the `MLE_MODULE` table and execute it. We can then use these functions in After Submit computations, validations, processes etc.

- `requireModule` is meant to be used with CommonJS (Node JS-like) modules. Example `var qrcode = requireModule('qrcode');`
- `loadScript` simply loads the script as is (IIFE or Browser-like). Example `loadScript('text-encoding');`

Some libraries are bundled in such a way that either way of loading will work. Some libraries on the other hand might need extra wrapping.

## Further notes

Some libraries can be used without a hitch (see validator). Some libraries need extra attention (see dompurify).

If a library is meant to be used in node-js, you might have to "browserify" it.
If a library is meant to be used in the browser, it might just work (validator), you might have to fake things like setTimeout or window (jimp), or you might have to fake a browser environment altogether (dompurify)

I am confident however that this process will get better with time, as MLE progresses, and more developers come up with better ways to bundle and ship libraries.
