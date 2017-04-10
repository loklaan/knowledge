# cross origin requests

Browsers want to know if your javascript is requesting http resources inside the intended scope of your page.

Imagine if, somehow, javascript was injected into your page and it performed all manner of expensive http traffic without your knowing... That's not nice.

The browser wants to prevent this so badly, that a spec, CORS, was put in place to make sure requests outside of your app's host & port need to be explicitly agreed upon on the other origin.

## browser opt-outs

For dev, it's often desirable to ignore CORS for awhile.

* **`XMLHttpRequest`** in most browsers will perform CORS with **no opt-out**.
* **`fetch`** will perform CORS, but **with an opt out**: `mode: 'no-cors'`
