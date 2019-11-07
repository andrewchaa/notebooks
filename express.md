# express.js

## Routing

#### Route Parameters

Route parameters are named URL segments that are used to capture the values specified at their position in the URL. The captured values are populated in the `req.params` object, with the name of the route parameter specified in the path as their respective keys.

```text
Route path: /users/:userId/books/:bookIdRequest URL: http://localhost:3000/users/34/books/8989req.params: { "userId": "34", "bookId": "8989" }
```



