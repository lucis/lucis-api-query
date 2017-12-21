## Visão geral
Se você usa o Mongoose como ORM em sua API, você pode ser relembrar de lidar com chamadas como:

    /monsters?color=purple&eats_humans=true

lucis-api-query lida com alguns desses trabalhos ocupados para você. Passe um *objeto de baunilha* (por exemplo, req.query) e as condições de consulta serão distribuídas para seus tipos apropriados de acordo com o seu esquema de mangustão. Por exemplo, se você tiver um booleano definido em seu esquema, converteremos o `eats_humans=true` para um booleano para pesquisa. Além de dar suporte a paginação, utilizando o mongoose-paginate

Ele também adiciona uma tonelada de operadores de pesquisa adicionais, como "menos do que", "maior do que", "não igual", "próximo" (para pesquisa geográfica), "em" e "tudo". Você pode encontrar uma lista completa abaixo.

Ao procurar seqüências de caracteres, por padrão, ele faz uma correspondência parcial, insensível a maiúsculas e minúsculas. (O que não é o padrão no MongoDB.)

## Uso

Aplique o plugin em qualquer esquema na forma habitual de mangustão:

```
monsterSchema.plugin (mongooseApiQuery);
```

Em seguida, chame como se estivesse usando `Model.find`. Isso retorna um Mongoose.Query:

```
Monster.apiQuery (req.query) .exec (...
```

Ou passe um retorno de chamada e ele executará `.exec` para você:

`` `
Monster.apiQuery (req.query, function (err, monsters) {...
`` `

## Exemplos

`t`,` y` e `1` são todos alias para` true`:

`` `
/monsters?eats_humans=y&scary=1
`` `

Corresponder em uma propriedade aninhada:

`` `
/monsters?foods.name=kale
`` `

Use a correspondência exata:

`` `
/monsters?foods.name={exact}KALE
`` `

Corresponde a `kale` ou` beets`:

`` `
/monsters?foods.name=kale,beets
`` `

Corresponde apenas onde `kale` e` betterets` estão presentes:

`` `
/monsters?foods.name={all}kale,beets
`` `

Operadores numéricos:

`` `
/monsters?monster_id={gte} 30&age={lt} 50
`` `

Combine operadores:

`` `
/monsters?monster_id={gte} 30 {lt} 50
`` `

geo perto, com raio (opcional) em milhas:

`` `
/monsters?latlon={near}38.8977,-77.0366
/monsters?latlon={near}38.8977,-77.0366,10
`` `

# Pagamento

`` `
/monsters?página=2
/ monsters?page=4&per_page=25 // per_page padrão para 10
`` `

##### Resultados da classificação

`` `
/monsters?sort_by=nome
/monsters?sort_by=nome, desc
`` `

##### Busca Schemaless

Você tem uma propriedade definida em seu esquema como `data: {}`, que pode ter algo dentro dele?Você também pode procurar isso e será tratado como uma string.

## Search Operators

Esta é uma lista dos operadores de pesquisa opcionais que você pode usar para cada SchemaType.

#### Número

- `number = {all} 123,456` - Ambos os 123 e 456 devem estar presentes
- `number = {nin} 123,456` - nem 123 nem 456
- `number = {in} 123,456` - 123 ou 456
- `number = {gt} 123` -> 123
- `number = {gte} 123` -> = 123
- `number = {lt} 123` - <123
- `number = {lte} 123` - <= 123
- `number = {ne} 123` - não 123
- `number = {mod} 10,2` - Onde (número / 10) tem restante 2

#### Corda

- `string = {all} match, batch` - Ambos coincidem * e * lote devem estar presentes
- `string = {nin} match, batch` - Nem combinação nem lote
- `string = {in} match, batch` - Qualquer combinação ou lote
- `string = {não} café` - Não café
- `string = {exacto} CoFeEe` - Correspondência exata sensível a maiúsculas e minúsculas de" CoFeEe "

#### Latlon

- `latlon = {próximo} 37, -122,5` Perto de 37, -122, com um raio máximo de 5 milhas
- `latlon = {próximo} 37, -122` Perto de 37, -122, sem limite de raio. Classifica automaticamente pela distância



## Para executar testes

`` `shell
node load_fixtures.js
node app.js
mocha
`` `

## Licença

MIT http://mit-license.org/