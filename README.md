# 5-Read-Operations

1. [Intro](#schema1)
2. [Understanding findOne() & find()](#schema2)
3. [Working with Comparison Operators.](#schema3)
4. [Querying Embedded Fields & Arrays](#schema4)


<hr>

<a name="schema1"></a>

## 1. Intro
!~~[read](./img/1read.png)

![read](./img/2read.png)

![read](./img/3read.png)
![read](./img/4read.png)
![read](./img/5read.png)

<hr>

<a name="schema2"></a>

## 2. Understanding findOne() & find()

En MongoDB, findOne y find son métodos utilizados para realizar consultas en una colección,
pero tienen diferencias clave en cómo devuelven los resultados:

**findOne:**

findOne se utiliza para recuperar un solo documento que coincida con los criterios de consulta especificados.
Devuelve un solo documento, o null si no se encuentra ninguna coincidencia.
Si hay varias coincidencias, findOne devuelve solo el primer documento que cumple con los criterios de consulta.
Ejemplo:

```
db.miColeccion.findOne({ campo: 'valor' });
```
Este ejemplo devolverá el primer documento de la colección miColeccion donde el campo sea igual a 'valor', 
o null si no se encuentra ninguna coincidencia.

**find:**

find se utiliza para recuperar un conjunto de documentos que coinciden con los criterios de consulta especificados.
Devuelve un cursor, que es un puntero a los documentos en la colección que cumplen con los criterios de consulta.
Puedes iterar sobre el cursor para acceder a todos los documentos que coinciden con la consulta.
Ejemplo:

```
const cursor = db.miColeccion.find({ campo: 'valor' });
cursor.forEach(document => {
  // Operaciones con cada documento que cumple con la consulta
});
```

Este ejemplo devuelve un cursor que apunta a todos los documentos en la colección miColeccion donde el campo es igual 
a 'valor'. Luego, puedes usar un bucle para iterar sobre los documentos y realizar operaciones con cada uno de ellos.

En resumen, findOne se utiliza cuando solo estás interesado en el primer documento que cumple con los criterios 
de consulta, mientras que find se utiliza cuando deseas obtener un conjunto de documentos que cumplen con 
los criterios y necesitas iterar sobre ellos. Ambos métodos son poderosos y se utilizan según los requisitos 
específicos de la aplicación.


<hr>

<a name="schema3"></a>

## 3. Working with Comparison Operators.

**Operadores de Comparación:**

- $eq: Igual a
- $ne: No igual a
- $lt: Menor que
- $lte: Menor o igual a
- $gt: Mayor que
- $gte: Mayor o igual a

```
db.movies.find({runtime:{$ne:60}}).pretty()
```
**Operadores Lógicos:**

- $and: Cumple con todos los criterios
- $or: Cumple con al menos uno de los criterios
- $not: No cumple con el criterio especificado
- $nor: No cumple con ninguno de los criterios

```
movieData> db.movies.findOne({runtime:{$in:[30,42]}})
```
```
movieData> db.movies.find({$or:[{"rating.average":{$lt:5}},{"rating.average":{$gt:9.3}}]})
```
```
movieData> db.movies.find({"rating.average":{$gt:9},genres:'Drama'}).count()
3
movieData> db.movies.find({$and:[{"rating.average":{$gt:9}},{genres:'Drama'}]}).count()
3
```
```
movieData> db.movies.find({runtime:{$ne:60}}).count()
70
movieData> db.movies.find({runtime:{$not:{$eq:60}}}).count()
70
```

**Otros Operadores:**

- $in: Coincide con alguno de los valores especificados en un array

- $nin: No coincide con ninguno de los valores especificados en un array

- $exists: Coincide con documentos que contienen o no contienen un campo específico
- $type: se utiliza para realizar consultas basadas en el tipo de datos de un campo en un documento. 
Permite buscar documentos donde un campo específico tenga un tipo de datos particular.
  - 1: Double (número de coma flotante)
  - 2: String (cadena de texto)
  - 3: Object (objeto BSON)
  - 4: Array (arreglo)
  - 8: Boolean (booleano)
  - 16: Integer (entero)
  - 17: Timestamp
  - 18: Long (entero largo)
  - Date: Date (fecha)
```
movieData> db.movies.find({runtime:{$type:'number'}}).count()
240

```
```
movieData> db.movies.find({name:{$exists: true,$eq:'Under the Dome'}})
```

**Evaluation Query Operators**

- $expr: Permite el uso de expresiones de agregación dentro del lenguaje de consulta.
- $jsonSchema: Valida documentos contra el JSON Schema proporcionado.
- $mod: Realiza una operación de módulo en el valor de un campo y selecciona documentos con un resultado específico.
- $regex: Selecciona documentos donde los valores coinciden con una expresión regular especificada.
- $text: Realiza búsquedas de texto.
- $where: Encuentra documentos que satisfacen una expresión JavaScript.

```
movieData> db.movies.find({summary:{$regex:/music/}}).count()
5

```


<hr>

<a name="schema4"></a>

## 4. Querying Embedded Fields & Arrays

Como vemos en este caso `rating: { average: 6.5 },` tenemos datos anidados.
A ver como sería la query para obtener ese valor.

```
movieData> db.movies.findOne()
{
  _id: ObjectId('6565bd02f59902f29c66619f'),
  id: 1,
  url: 'http://www.tvmaze.com/shows/1/under-the-dome',
  name: 'Under the Dome',
  type: 'Scripted',
  language: 'English',
  genres: [ 'Drama', 'Science-Fiction', 'Thriller' ],
  status: 'Ended',
  runtime: 60,
  premiered: '2013-06-24',
  officialSite: 'http://www.cbs.com/shows/under-the-dome/',
  schedule: { time: '22:00', days: [ 'Thursday' ] },
  rating: { average: 6.5 },
  weight: 91,
  network: {
    id: 2,
    name: 'CBS',
    country: { name: 'United States', code: 'US', timezone: 'America/New_York' }
  },
  webChannel: null,
  externals: { tvrage: 25988, thetvdb: 264492, imdb: 'tt1553656' },
  image: {
    medium: 'http://static.tvmaze.com/uploads/images/medium_portrait/0/1.jpg',
    original: 'http://static.tvmaze.com/uploads/images/original_untouched/0/1.jpg'
  },
  summary: "<p><b>Under the Dome</b> is the story of a small town that is suddenly and inexplicably sealed off from the rest of the world by an enormous transparent dome. The town's inhabitants must deal with surviving the post-apocalyptic conditions while searching for answers about the dome, where it came from and if and when it will go away.</p>",
  updated: 1529612668,
  _links: {
    self: { href: 'http://api.tvmaze.com/shows/1' },
    previousepisode: { href: 'http://api.tvmaze.com/episodes/185054' }
  }
}
```
Sería `rating.average` porque los datos están así en la bbdd `rating: { average: 6.5 },`

```
movieData> db.movies.find({"rating.average":{$gt:7}})
```
Pero en cambio es el caso de `genres` los datos son un array `genres: [ 'Drama', 'Science-Fiction', 'Thriller' ]` y la
query sería asi: 

```
movieData> db.movies.findOne({genres:['Drama']})

```