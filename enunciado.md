# Práctica WEB-API/Node.js/MongoDB

Bootcamp Web5 2018

## Imaginemos que un cliente nos pasa el siguiente briefing para que le hagamos este trabajo:

Desarrollar el software que se ejecutará en el servidor dando servicio a una app (API) de venta de artículos de segunda mano, llamada Nodepop. Hazte a la idea que esta API que vas a construir será utilizada por una
app iOS y otra Android.

La pantalla principal de la (supuesta) app mostraría una lista de anuncios y permitiría tanto buscar como poner filtros por varios criterios, por tanto la API a desarrollar deberá proveer los métodos necesarios para esto.

## Cada anuncio mostrará los siguientes datos:

+ Nombre del artículo, un anuncio siempre tendrá un solo artículo
+ Si el artículo se vende o se busca
+ Precio. Será el precio del artículo en caso de ser una oferta de venta. En caso de que sea un anuncio de ‘se busca’ será el precio que el solicitante estaría dispuesto a pagar
+ Foto del artículo. Cada anuncio tendrá solo una foto.
+ Tags del anuncio. Podrá contener uno o varios de estos cuatro: work, lifestyle, motor y mobile
  
## Operaciones que debe realizar el API a crear:

+ Lista de anuncios paginada. Con filtros por tag, tipo de anuncio (venta o búsqueda), rango de precio (precio min. y precio max.) y nombre de artículo (que empiece por el dato buscado)
+ Lista de tags existentes
+ Creación de anuncio
  
Los sistemas donde se desplegará el API utilizan bases de datos MongoDB.

El API recibirá bastantes peticiones en algunos momentos del día, especialmente los fines de semana, por tanto queremos que aproveche lo mejor posible los recursos del servidor donde estará instalado.

Se solicita que el entregable venga acompañado de una mínima documentación y el código del API esté bien formateado para facilitar su mantenimiento. En esta fase, ya que se desea probar si el modelo de negocio va a funcionar, no serán necesarios ni tests unitarios ni de integración.

El site donde se despliegue tendrá una lista de anuncios con filtros en su página principal. Obtendrá información de la base de datos usando los modelos y la mostrará.

Para mostrar los datos será suficiente con una página renderizada en servidor EJS que muestre la lista de anuncios, en la que si ponemos filtros en la URL los aplique, algo como lo que hicimos en clase con el API.

Por ejemplo si hacemos una petición a

<http://localhost:3000/anuncios?start=1&limit=3&sort=name&tag=lifestyle)http://localhost:3000/anuncios?start=1&limit=3&sort=name&tag=lifestyle>

mostraría los anuncios correspondientes a esos filtros.

# Notas para el desarrollador

## Cómo empezar

El orden de las primeras tareas podría ser:

+ Crear app Express y probarla (express nodepop --ejs)
+ Instalar Mongoose, modelo de anuncios y probarlo (con algún anuncio.save por ejemplo)
+ Hacer un script de inicialización de la base de datos, que cargue el json de anuncios. Se puede llamar p.e. install_db.js, debería borrar las tablas y cargar anuncios, y algún usuario. Lo podemos poner en el package.json para poder usar npm run installDB. Referencias:
    1. Cargar vuestro módulo connectMongoose.js y vuestros modelos, luego usad

        ```javascript
        conn.once('open', () => { … para empezar el proceso
        ```
    2. <http://mongoosejs.com/docs/api.html#query_Query-deleteMany>
    3. <http://mongoosejs.com/docs/api.html#model_Model.insertMany>
    4. Estas operaciones deberán hacerse una detrás de la otra, o dicho de otro modo, cuando termine la primera se lanzará la segunda. Para esto podéis usar callbacks, promesas (si miráis la doc veréis que devuelven una promesa!) o promesas con async/await (recomendado).
+ Hacer un fichero README.md con las instrucciones de uso puede ser una
muy buena idea, lo ponemos en la raíz del proyecto y si apuntamos ahí
como arrancarlo, como inicializar la BD, etc nos vendrá bien para cuando
lo olvidemos o lo coja otra persona
+ Hacer una primera versión básica del API, por ejemplo GET
/apiv1/anuncios que devuelva la lista de anuncios sin filtros.
+ Crear la página de inicio del site y sacar la lista de anuncios
+ Mejorar la lista de anuncios poniendo filtros, paginación, etc
+ A partir de aquí ya tendríamos mucho hecho!

## Detalles útiles

Tras analizar el briefing vemos que tenemos que guardar cosas en la base de datos, como por ejemplo los anuncios.

Por tanto, nos podemos hacer el modelos de mongoose con esta definición.

```javascript
var anuncioSchema = mongoose.Schema({
nombre: String,
venta: Boolean,
precio: Number,
foto: String,
tags: [String]
});
```

Nos vendrá bien hacer un script de inicialización de la base de datos, que podemos llamar install_bd.js. Este script debería borrar las tablas si existen y cargar un fichero llamado anuncios.json que tendrá un contenido similar a este:

```javascript
// anuncios.json
{
    "anuncios": [
        {
            "nombre": "Bicicleta",
            "venta": true,
            "precio": 230.15,
            "foto": "bici.jpg",
            "tags": [ "lifestyle", "motor"]
        },
        {
            "nombre": "iPhone 3GS",
            "venta": false,
            "precio": 50.00,
            "foto": "iphone.png",
            "tags": [ "lifestyle", "mobile"]
        }
]
}
```

Podéis añadir más anuncios si queréis.

Las fotos podéis hacerlas con el móvil o sacarlas de algún banco fotográfico gratuito… el API tendrá que devolver las imágenes, por ejemplo de la carpeta /public/images/<nombreRecurso>, por tanto obtendriamos una imagen haciendo una petición en la url <http://localhost:3000/images/anuncios/iphone.png>

## Enviar arrays con Postman

Práctica BootWeb5 JS-Node-Mongo.bmp

## Lista de anuncios

Lista de anuncios paginada.

Filtros:
+ por tag, tendremos que buscar incluyendo una condición por tag
+ tipo de anuncio (venta o búsqueda), podemos usar un parámetro en query string llamado venta que tenga true o false
+ rango de precio (precio min. y precio max.), podemos usar un parámetro en la query string llamado precio que tenga una de estas combinaciones:
    1. 10-50 buscará anuncios con precio incluido entre estos valores 
        ```javascript
        { precio: { '$gte': '10', '$lte': '50' } }
        ```

    2. 10+ buscará los que tengan precio mayor que 10
        ```javascript
        { precio: { '$gte': '10' } }
        ```
    3. -50 buscará los que tengan precio menor de 50
        { precio: { '$lte': '50' } }

○ 50 buscará los que tengan precio igual a 50 { precio: '50' }
+ nombre de artículo, que empiece por el dato buscado en el parámetro
nombre. Una [expresión
regular](http://docs.mongodb.org/manual/reference/operator/query/regex/#perform-case-insensitive-regular-expression-match)
nos puede ayudar filters.nombre = new RegExp('^'

+ req.query.nombre, "i");

Para recibir la lista de anuncios, la llamada podría ser una como esta:
[GET
http://localhost:3000/apiv1/anuncios?](http://localhost:3000/apiv1/anuncios?tag=mobile&venta=false&no)[tag](http://localhost:3000/apiv1/anuncios?tag=mobile&venta=false&no)[=mobile&](http://localhost:3000/apiv1/anuncios?tag=mobile&venta=false&no)[venta](http://localhost:3000/apiv1/anuncios?tag=mobile&venta=false&no)[=false&](http://localhost:3000/apiv1/anuncios?tag=mobile&venta=false&no)no
mbre=ip&precio=50-&start=0&limit=2&sort=precio&token=eyJ0eXAiO
iJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJfaWQiOiI1NWZkOWFiZGE4Y2QxZDlhMj
QwYzgyMzEiLCJub21icmUiOiJhZG1pbiIsImVtYWlsIjoiamFtZzQ0QGdtYWls
LmNvbSIsImNsYXZlIjoiMTIzIiwiX192IjowfQ.y8wPJhNaS8Vf51ZlX9qZBlr
TLGGy4JzDgN2eGSHeQfg

![](Práctica%20BootWeb5%20JS-Node-Mongo/Image_002.jpg)

Documentación y calidad de código

Como nos piden algo de documentación podemos usar la página index de
nuestro proyecto o un fichero README.md para escribir la documentación
del API, y los más valientes pueden probar a hacerlo con
[iodocs](https://github.com/mashery/iodocs).
![](Práctica%20BootWeb5%20JS-Node-Mongo/Image_003.jpg)
En cuanto a la calidad de código, será un punto a nuestro favor que lo
validemos con ESLint
![](Práctica%20BootWeb5%20JS-Node-Mongo/Image_004.png)
([http://eslint.org/](http://eslint.org/)), que auna revisión de estilo
de código y de posibles bugs. En ESlint podemos crear un fichero para
definir las reglas con:
![](Práctica%20BootWeb5%20JS-Node-Mongo/Image_005.png)

$ npm install eslint

$ ./node_modules/.bin/eslint --init

{

"node": true, "esnext": true, "globals": {}, "globalstrict": true,
"quotmark": "single", "undef": true, "unused": true

}

Esto debería haberte creado un fichero .eslintrc para que vayas
añadiendo las reglas que quieras. Para saber más echa un vistazo a
[https://eslint.org/docs/user-guide/getting-started](https://eslint.org/docs/user-guide/getting-started)
Ver
[https://www.sitepoint.com/comparison-javascript-linting-tools/](https://www.sitepoint.com/comparison-javascript-linting-tools/)
![](Práctica%20BootWeb5%20JS-Node-Mongo/Image_006.png)
Si las utilidades como ESLint las metemos como scripts de NPM
([http://www.jayway.com/2014/03/28/running-scripts-with-npm/](http://www.jayway.com/2014/03/28/running-scripts-with-npm/))
nos será muy fácil pasarlas con frecuencia.
