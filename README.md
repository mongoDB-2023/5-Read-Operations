# 5-Read-Operations

1. [Intro](#schema1)
2. [Understanding findOne() & find()](#schema2)
3. [Working with Comparison Operators.](#schema3)




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








