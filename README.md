# Supuesto práctico Chakray 2020 Adrián Ródenas Picó
En este documento se explicará como configurar el entorno docker proporcionado por Chakray, para que tenga la solución proporcionada por Adrián Ródenas Picó.

## Estructura del zip
El zip que contiene la solución consta de los siguientes elementos:
- **Carpeta "elementos_configuracion"**: contiene todos los ficheros de configuración y soluciones realizadas para el supuesto práctico. Que son: "data_food_db.dbs" para el datasource, "API.xml" para el código apache synapse y "swaggerApi.json" para la configuración de la API en Api Manager

- **Carpeta imagenes**: contiene imagenes de configuración que se mostrarán durante este documento como apoyo visual.
- **Carpeta docker_composer_wso2** : contiene lo necesario para ejecutar el entorno del supuesto. *Nota:* se ha alterado el documento *docker-compose.yml* para añadir un par de volúmenes de docker para poder conservar los datos durante la ejecución de la práctica

## Instalación de los ficheros de configuración y soluciones
Lo primero que se debe hacer después de descargar fichero zip,
es acceder a la carpeta **docker_composer_wso2** con el terminal y ejecutar el comando **docker-compose up** como se muestra en la imagen.

![imagen ejecución docker](/imagenes/dockerEjecucion.JPG)

Con el entorno ya en funcionamiento, se procede a configurar la solución. Se empezará con la instalación del datasource.

### Configuración del datasource

El fichero necesario para esto es **data_food_db.dbs** dentro de la carpeta "elementos_configuracion". Para poder instalarlo se siguen los siguientes pasos:

1. Se accede a la url https://127.0.0.1:9444/carbon/admin/login.jsp (o https://172.28.1.3:9444/carbon/admin/login.jsp) y se ingresa el usuario (admin) y contraseña (admin).
2. Una vez logueado, se realiza click en "services->DataService->upload". ![localización del menú de upload](/imagenes/menuIntroducirDatasource.JPG)
3. Se introduce el fichero **data_food_db.dbs** y se da click a upload.

### Configuración de la API (Enterprise Integrator)

El fichero necesario para esto es **API.xml** dentro de la carpeta "elementos_configuracion". Se siguen los siguientes pasos:
1. Sin salir de la url actual, se hace click en el menú API. Luego se hace clik en "+ Add API" ![localizacion del menú de Api](/imagenes/menuApi.JPG)
2. Al pulsar este enlace, se despliegan las opciones de configuración de la Api. Se hace click en *"switch to source view"* ![localización lugar para acceder a editor de texto](/imagenes/insertarApi.JPG)
3. Ahora se muestra un editor de texto, se coge el código de **API.xml** y se pega reemplazando el código que haya actualmente.![imagen con el código de la API](/imagenes/codigoApi.JPG)
4. Se pulsa el botón salvar. 

Con esto se saldría del menú de creación de API, habiéndose creado la misma como se muestra en la imagen.
![resultado de la Api](/imagenes/resultadoApi.JPG)

### Configuración de la API (Api Manager)

El fichero necesario para esto es **swaggerApi.json** dentro de la carpeta "elementos_configuracion". Se siguen los siguientes pasos:

1.	Se entra en la url https://127.0.0.1:9443/carbon/admin/login.jsp (publisher) y se introduce el usuario “admin” y la contraseña “admin”.
2.	Se pulsa en el enlace “+ ADD NEW API”. ![publisher inicio](/imagenes/publisherAddApi.JPG)
3. Dentro de este menú, se pulsa la primera opción **I Have and existing API** y se desplegarán dos opciones. Se escoge **Swagger File**. Saldrá una opción para buscarlo en el escritorio, buscamos el fichero **swaggerApi.json**.
![configuración 1.5 publisher](/imagenes/apiSwagger.jpg)
4. Se sigue el wizard de creación de la API. En la primera ventana solo se tendrá que añadir el contexto. Luego se hace click en el botón azul "Next: Implementation" ![configuración primera parte publisher](/imagenes/contextoApi.JPG)
5. Se pulsa en el desplegable "Managed API" y se introduce la configuración de la imagen. Luego se hace click en el botón azul "Next: Manage" ![configuración segunda parte publisher](/imagenes/datosApi.png)
6. Para finalizar, se introduce la configuración de la imagen y se pulsa en "Save & Publish" ![configuración tercera parte publisher](/imagenes/finalizarApi.png)

Con esto finalizaría la parte de la configuración e instalación de la solución.

## Operaciones de la Api
Recursos de la API
- solicitarPedido: Se encarga de hacer un pedido a la base de datos. Se le debe enviar un mensaje JSON como se muestra a continuación:
```JSON
{
   "USER":"Rodenas",
   "EMAIL":"adrian.ropi@gmail.com",
   "PEDIDOS":[
      {
         "PRODUCTO":3,
         "CANTIDAD":2
      },
      {
         "PRODUCTO":4,
         "CANTIDAD":1
      }
   ]
}
```
- verCatalogo: Devuelve un listado de los productos de la empresa, compuesto por identificador del producto y nombre del producto.
- actualizarPedido: Un pedido tiene 3 estados, este recurso admitirá el identificador de estado y el ID del pedido. Los estados son ‘Solicitado’, ‘Repartiendo’ y ‘Entregado’. Se le debe enviar el siguiente JSON:
```JSON
{
   "ID":"f4a3e990-7918-495f-9a19-2daf3e379e23",
   "STATE":" Repartiendo’"
}
```
- cancelarPedido/{ID}: Permite cancelar un pedido en base a un identificador dado.
- verPedido/{ID}: Permite ver los datos del pedido realizado.

![recursos imágen](/imagenes/recursos.png)


## Objetivos alcanzados del supuesto práctico.
- Implementar los métodos requeridos.
- Autorización para el acceso de la Api y logueo.

## Elementos de mejora.
- Añadir validaciones y mejoras en como se muestra el resultado verPedido/{ID}.

