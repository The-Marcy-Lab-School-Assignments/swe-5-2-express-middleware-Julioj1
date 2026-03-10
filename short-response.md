# Short Response Questions

Answer each question below in your own words. Aim for 3–5 sentences per answer. Be specific — use the exact terms and concepts from the lesson.

Your responses will each be evaluated out of 6 points. You can earn 3 points for writing quality and 3 points for the accuracy and precision of the technical content per question.

---

## Question 1: Express vs `node:http`

Express is described as a framework that "wraps" `node:http`. What does that mean? Compare how you would handle a `GET /api/users` request in `node:http` versus in Express. What does Express do for you automatically that you had to write manually with `node:http`?

**Your answer here**:

When we say Express “wraps” node:http, it means Express is built on top of Node’s native http module and uses it internally.
Instead of manually handling low-level request parsing, routing logic, headers, and responses, Express hides much of that boilerplate and gives you simpler methods like `app.get()` and `res.json()`.

**Cleaner structure** – No large `if` statements for every route.
**Middleware system** – Built-in structure for handling logging, parsing JSON, authentication, etc.
**Query and parameter parsing** – Express parses route params and query strings automatically.

## Question 2: Endpoints, Controllers, and Middleware

What are **controllers** and **middleware** in Express? What are each responsible for and how do they work together to handle incoming requests?

**Your answer here**:

**Controllers** and **middleware** are both functions that help handle incoming HTTP requests

A controller is responsible for the main business logic of a route. It decides what happens when a request reaches its final destination.
Like retrieve data from a database or an array or sending the response back to the client.

Middleware functions run before the controller they can validate data, log requests, check authentication. It's what happens before we handle the request.
Also Middleware must call `next()` to mo ve forward or send a response to stop the request cycle.

## Question 3: Query Strings and Route Parameters

How are **query strings** and **route parameters** similar? How are they different? In your answer, provide an example of when you would use each.

**Your answer here**:

Both **query strings** and **route parameters** are ways to send additional information from the client to the server through the **URL**. They allow the server to receive dynamic input without changing the route structure completely. In Express, both are accessed through the `req` object:

- Route parameters - `req.params`

- Query strings - `req.query`

### Route Parameters

**Route parameters** are part of the URL path itself and are typically used to identify a specific resource.

```js
const singleQuote = (req, res) => {
  const { id } = req.params;
  const quote = quotes.find((quote) => quote.id === Number(id));

  if (!quote) {
    res.status(404).send({ error: `No quote with id ${id}` });
    return;
  }

  res.json(quote);
};
```

If a user visits: `/api/quotes/4` `4` is the route parameter.

### Query Strings

Query strings come after a `?` in the URL and are typically used to filter, sort, or modify results within a collection.

```js
const returnQuotes = (req, res) => {
  const { topic } = req.query;
  if (topic) {
    const filtered = quotes.filter((quote) => quote.topic === topic);
    res.json(filtered);
  } else {
    res.json(quotes);
  }
};
```

If a user visits: `/api/quotes?topic=science` `topic=science` is the query string.

## Question 4: Same-Origin Requests

For API fetch calls from a client-side application, explain the difference between fetching from endpoints with relative paths like `/api/quotes` and fetching from endpoints with a full URL like `https://dog.ceo/api/breeds/image/random`. Why do we not send a fetch using a url like `http://localhost:8080/api/quotes`?

**Your answer here**:

When making API calls from a client-side application, the difference between relative paths and full URLs is about where the request is being sent and how the browser determines the destination.

If your app is running on `http://localhost:8080` the browser automatically sends the request to `http://localhost:8080/api/quotes` you dont need to specify the full URL because the browser already knows the origin.

A full URL includes the protocol and domain. It is used when fetching data from an external API on a different server.
`https://dog.ceo/api/breeds/image/random` This is necessary because the request is going to a completely different origin.

### Why We Don’t Use `http://localhost:8080/api/quotes`

Even though this works in development, it is not good practice because if you deploy your app to `http://myapp.com` the hardcoded URL will break,
production servers won't run on `localhost:8080`, and relative paths allow the browser to dynamically use whatever origin is currently serving the page.
