## Instrucciones

### Iteración 1

Primero, tendrás que importar la base de datos que utilizaremos en esta práctica. Vamos a utilizar la base de datos de Crunchbase. Crunchbase es el principal destino para descubrir las tendencias de la industria, inversiones y noticias sobre cientos de miles de empresas a nivel mundial, desde nuevas empresas hasta las empresas más top a nivel de riquezas. Crunchbase es reconocida como la principal fuente información sobre empresas por millones de usuarios.

La base de datos contiene más de 18k de documentos, y cada uno de ello contiene una gran cantidad de información de cada compañía. Un ejemplo del documento es el siguiente:

![image](https://user-images.githubusercontent.com/23629340/36494916-d6db1770-1733-11e8-903e-5119b3c1b688.png)

1. Deberás descargar el zip de la base de datos en la ruta del repositorio.
2. Descomprime el archivo.
3. Navega hasta la carpeta del repositorio en la terminal e introduce el siguiente comando para importar la base de datos:

```bash
$ mongoimport --db companies --collection companies --file companies.json
```

\_Nota: en caso de errores o de que el comando no responda, añade [--jsonArray] al final del comando anterior.

4. Comprueba en Mongo Compass si se ha importado correctamente.

![image](https://user-images.githubusercontent.com/23629340/36534191-1f1bc5ec-17c6-11e8-9463-4945679b98c0.png)

### Iteración 2.

Ahora estás listo para empezar a trabajar. Deberás realizar las siguiente búsquedas:

1. Todas las compañías cuyo nombre coincida con 'Babelgum'. Devuelve solo el campo `name`.

```
{
 filter: {
  name: 'Babelgum'
 },
 project: {
  name: 1
 }
}
```

2. Todas las compañías que tengan más de 5000 empleados. Limita la búsqueda a 20 compañías y devuelve la información ordenada descendentemente por el **número de empleados** (`number_of_employees`)

```
{
 filter: {
  number_of_employees: {
   $gte: 5000
  }
 },
 sort: {
  number_of_employees: -1
 },
 limit: 20
}
```

3. Todas las compañías que hayan sido fundada entre el año 2000 y 2005, ambos incluídos. Devuelve sólo el campo `name` y el `founded_year`

```
{
 filter: {
  founded_year: {
   $gte: 2000,
   $lte: 2005
  }
 },
 project: {
  name: 1,
  founded_year: 1,
  _id: 0
 }
}
```

4. Todas las compañías cuyo importe de valoración sea mayor que 100.000.000 y que haya sido fundada antes de 2010. Devuelve solo los campos `name` y `ipo`

```
{
 filter: {
  $and: [
   {
    'ipo.valuation_amount': {
     $gt: 100000000
    }
   },
   {
    founded_year: {
     $lt: 2010
    }
   }
  ]
 },
 project: {
  ipo: 1,
  name: 1
 }
}
```

5. Ordena todas las compañías por el campo `ipo` en orden descendente.

```
{
 sort: {
  ipo: -1
 }
}
```

6. Todas las compañías que no incluyan el campo `partners`.

```
{
 filter: {
  partners: {
   $exists: 0
  }
 }
}
```

7. Todas las compañías que tengan al menos 100 empleados pero menos de 1000. Devuelve sólo el campo `name` y `number_of_employees`

```
{
 filter: {
  number_of_employees: {
   $gte: 100,
   $lt: 1000
  }
 },
 project: {
  name: 1,
  number_of_employees: 1,
  _id: 0
 }
}
```

8. Todas las compañías que tengan menos de 1000 empleados y que hayan sido fundadas antes de 2005. Ordénalas por el número de empleados ascendentemente y limita la busqueda a 10 compañías.

```
{
 filter: {
  $and: [
   {
    number_of_employees: {
     $lt: 1000
    }
   },
   {
    founded_year: {
     $lt: 2005
    }
   }
  ]
 },
 sort: {
  number_of_employees: 1
 },
 limit: 10
}
```

9. Recuperas las 10 compañías con más empleados ordenados por `number_of_employees` descendentemente.

```
{
 sort: {
  number_of_employees: -1
 },
 limit: 10
}
```

10. Todas las compañías fundadas en el segundo semestre del año. Limita la búsqueda a 1000 empresas.

```
{
 filter: {
  founded_month: {
   $gt: 6
  }
 },
 limit: 1000
}
```

11. Todas las compañías fundadas antes del 2000 que tengan un valor de adquisición mayor que 10.000.000.

```
{
 filter: {
  $and: [
   {
    founded_year: {
     $lt: 2000
    }
   },
   {
    'acquisition.price_amount': {
     $gt: 10000000
    }
   }
  ]
 }
}
```

12. Todas las compañías adquiridas (acquired) después de 2010 ordenadas por la cuenta de adquisición (amount_acquisition) y devuelve sólo su `nombre` y el campo `acquisition`.

```
{
 filter: {
  'acquisition.acquired_year': {
   $gt: 2010
  }
 },
 project: {
  name: 1,
  acquisition: 1,
  _id: 0
 },
 sort: {
  'acquisition.price_amount': -1
 }
}
```

13. Ordena las empresas por orden de fundación y devuelve sólo el nombre y el año de fundación.

```
{
 filter: {
  founded_year: {
   $gt: 0
  }
 },
 project: {
  name: 1,
  founded_year: 1,
  _id: 0
 },
 sort: {
  founded_year: 1,
  founded_month: 1,
  founded_day: 1
 }
}
```

14. Todas las compañías que sean de la categoría web y tengan más de 4000 empleados. Ordenalos por número de empleados en orden ascendente.

```
{
 filter: {
  $and: [
   {
    category_code: 'web'
   },
   {
    number_of_employees: {
     $gt: 4000
    }
   }
  ]
 },
 sort: {
  number_of_employees: 1
 }
}
```

Happy Coding!
