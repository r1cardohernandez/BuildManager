# Documentación de la API de VentasReservas

## Introducción

La **API de Ventas y Reservas** gestiona las operaciones relacionadas con ventas y reservas de unidades. Permite recuperar, crear, actualizar y eliminar registros de ventas y reservas. La API está integrada con AWS Lambda para el manejo de solicitudes.

* [Ver Swagger](contrato/API_VentasReservas.yaml "ver capacidad")

## Seguridad
- **Esquema de Seguridad:**
    - **Nombre:** projectAuthorization
    - **Tipo:** apiKey
    - **Ubicación:** header
    - **Descripción:** Autenticación mediante AWS Cognito. Incluir el token JWT en el encabezado Authorization.
    - **Tipo de Autorizador:** cognito_user_pools
    - **ARN del User Pool:** arn:aws:cognito-idp:us-east-2:941377161031:userpool/us-east-2_gFRspxKNP

### Autenticación y Autorización

- **Método de Autenticación:** La API utiliza JWT para autenticar las solicitudes. El token JWT debe ser incluido en el encabezado `Authorization` de la solicitud.
- **Header de Autenticación:**
  - **Nombre del encabezado:** `Authorization`
  - **Formato:** `Bearer <token>`

## Servidores

- **Base URL:** `https://b93gc35as1.execute-api.us-east-2.amazonaws.com/{basePath}`
  - **Variable de URL:** `basePath` (default: `DEV`)

## Endpoints

### 1. getSalesReserve (Obtener Ventas y Reservas)

- **URL:** `/ventas-reservas`
- **Método HTTP:** GET
- **Descripción:** Recupera una lista de todas las ventas y reservas.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/getSalesReserveResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getSalesReserve`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getSalesReserve/invocations`
  - **Descripcion:** Recupera todos los registros de ventas y reservas desde la base de datos.
  - **Operacion DynamoDB:** `Scan`
    - **Descripcion:** Escanea la tabla DynamoDB y devuelve todos los ítems. Se puede aplicar un filtro si es necesario.

### 2. createProject (Crear Venta o Reserva)

- **URL:** `/ventas-reservas`
- **Método HTTP:** POST
- **Descripción:** Crea una nueva venta o reserva.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/createSalesReserveRequest.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 201 Created
  - **Formato de Respuesta:** Se muestra un mensaje informativo indicando q se creo un item nuevo `"message": "Venta o reserva creada exitosamente"`
- **Funcion lambda**  `createSalesReserve`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=createSalesReserve/invocations`
  - **Descripcion:**  Crea un nuevo registro de venta o reserva en la base de datos.
  - **Operacion DynamoDB:** `PutItem`
    - **Descripcion:** Inserta un nuevo ítem en la tabla DynamoDB.

### 3. getSalesReserveID (Obtener Venta o Reserva por ID)

- **URL:** `/ventas-reservas/{ventaReservaID}`
- **Método HTTP:** GET
- **Descripción:** Recupera la información de una venta o reserva específica por ID.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `ventaReservaID` (string) - ID de la venta o reserva a consultar.
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/getSalesReserveIDResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getSalesReserveID`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=getSalesReserveID/invocations`
  - **Descripcion:** Recupera la información de un proyecto específico por ID.
  - **Operacion DynamoDB:** `GetItem`
    - **Descripcion:** Recupera un registro específico de venta o reserva por ID.

### 4. updateSalesReserve (Actualizar Venta o Reserva)

- **URL:** `/ventas-reservas/{ventaReservaID}`
- **Método HTTP:** PUT
- **Descripción:** Actualiza un registro existente de venta o reserva.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `ventaReservaID` (string) - ID de la venta o reserva a consultar.
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/updateSalesReserveRequest.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se confirma mediante un mensaje que fue actualizado el proyecto indicado.
    <a href="ejemplo/updateSalesReserveResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `updateSalesReserve`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=updateSalesReserve/invocations`
  - **Descripcion:** Actualiza un registro de venta o reserva existente en la base de datos
  - **Operacion DynamoDB:** `UpdateItem`
    - **Descripcion:** Actualiza un ítem existente en la tabla DynamoDB usando ventaReservaID como clave de partición.

### 5. deleteSaleReserve (Eliminar Venta o Reserva)

- **URL:** `/ventas-reservas/{ventaReservaID}`
- **Método HTTP:** DELETE
- **Descripción:** Elimina un registro existente de venta o reserva.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `ventaReservaID` (string) - ID de la venta o reserva a consultar.
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 204 No Content
  - **Formato de Respuesta:** No hay formato de respuesta debido a que se trata de una respuesta 204 q confirma la solicitud exitosa pero sin body o carga util en respuesta
- **Funcion lambda**  `deleteSaleReserve`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:deleteSaleReserve/invocations`
  - **Descripcion:** Elimina un registro específico de venta o reserva de la base de datos.
  - **Operacion DynamoDB:** `DeleteItem`
    - **Descripcion:** Elimina un ítem específico de la tabla DynamoDB usando ventaReservaID como clave de partición.

## Estructura de Datos en DynamoDB
A continuación se muestra la definición de la tabla VentasReservas en DynamoDB. Esta tabla almacena información sobre las ventas y reservas de unidades de los proyectos existentes y está estructurada de la siguiente manera

| Campo | Tipo | Descripcion |
|:-:|:-:|:-:|
|VentaReservaID|String|Identificador único para la venta o reserva.|
|ClienteID|String|Identificador único del cliente asociado a la transacción.|
|Comision|Number|Comisión aplicada en la transacción.|
|Estado|String|Estado de la transacción, como Reservada o Vendida.|
|FechaTransaccion|String|Fecha en que se realizó la transacción, en formato `YYYY-MM-DD`.|
|ProjectID|String|Identificador único del proyecto asociado.|
|TipoTransaccion|String|Tipo de transacción, como `Venta` o `Reserva`.|
|UnidadID|String|Identificador único de la unidad asociada.|
|


