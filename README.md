# lucis-api-query

## Motivação
This library merge three other libraries used for paginating and querying resources in Mongoose:  `mongoose-paginate`, `mongoose-api-query` and.

`mongoose-paginate` works creating a query in the database using pagination's parameters.
`express-paginate` parses the parameters coming from the request's query params, among other things.
`mongoose-api-query` empowers mongoose's query api including helpers and regex' like functionality.
## How to install?

### Mongoose
In your schema/model file you just need to import `lucis-api-query` and add as a plugin to your Mongoose Schema. Just like that

```javascript
import { pluginMongoose } from 'lucis-api-query';

const songSchema = new Schema({
    name: String
});

songSchema.plugin(pluginMongoose);

module.exports = mongoose.model('Song', songSchema);
```

## Using
In your router, just replace the handler function with `Model.lucisApiQuery()`. You can provide a callback `(req, res, data)`, but it will work fine if you don't. I will probably add some options for this functions, but right now the request holds responsability about `select`, `populate`, `limit` (actually we override it if it gets too big).

```javascript
app.get('/songs', Song.lucisApiQuery());
```
