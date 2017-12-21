# lucis-api-query

## Motivação
Essa lib contempla três tecnologias necessárias para a paginação funcionar, o `mongoose-paginate`, o `mongoose-api-query` e o `express-paginate`. O `mongoose-paginate` funciona se atrelando ao Model do Mongoose e facilitando a busca pelas entidades. Já o `express-paginate` funciona como um middleware que trata os dados enviados pelo cliente para a paginação funcionar corretamente. Também há duas funções que eu mesmo criei para normalizar como os dados de paginação são criados.

## Como implantar?

### Mongoose
Você deve criar um modelo no Mongoose da entidade da qual você deseja implantar a paginação. No próprio schema você deve adicionar o plugin da biblioteca.

```javascript
import { pluginMongoose } from 'lucis-api-query';

const songSchema = new Schema({
    name: String
});

songSchema.plugin(pluginMongoose);

module.exports = mongoose.model('Song', songSchema);
```

### Express
