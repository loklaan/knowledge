# http server

Basic example of a http server in core http.

```js
const http = require('http')

const server = http.createServer((request, response) => {
  console.log(request.url)
  response.end('Hello Node.js Server!')
})

const listener = server.listen(
  process.env.PORT || 3000,
  (err) => {
    if (err) {
      return console.log('😭', err)
    }

    console.log(`server is listening on ${listener.address().port}`)
  }
)
```