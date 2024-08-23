# Documentación de la API de Unidades

## Introducción

La **API de Unidades** permite gestionar unidades de vivienda almacenadas en DynamoDB. La API ofrece funcionalidades para obtener, crear, actualizar y eliminar unidades. Está diseñada para integrarse con Lambda y utiliza AWS Cognito para autenticación.

* [Ver Swagger](contrato/API_Unidades.yaml "ver capacidad")

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

- **Base URL:** `https://1hy9wgxhwb.execute-api.us-east-2.amazonaws.com/{basePath}`
  - **Variable de URL:** `basePath` (default: `Dev`)

## Endpoints

### 1. getUnits (Obtener Todas las Unidades)

- **URL:** `/unidades`
- **Método HTTP:** GET
- **Descripción:** Recupera una lista de todas las unidades almacenadas en DynamoDB.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplos/getUnitsResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getProjects`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getUnits/invocations`
  - **Descripcion:** Recupera todas las unidades existentes de todos los proyectos desde la tabla DynamoDB.
  - **Operacion DynamoDB:** `Scan`
    - **Descripcion:** Escanea la tabla DynamoDB y devuelve todos los ítems. Se puede aplicar un filtro si es necesario.

### 2. createUnit (Crear una nueva unidad)

- **URL:** `/unidades`
- **Método HTTP:** POST
- **Descripción:** Crea una nueva unidad en un proyecto existente
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplos/createUnitRequest.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 201 Created
  - **Formato de Respuesta:** Se muestra el mismo body tanto en el request como en el response validadno la inforamcion subida.
    <a href="ejemplos/createUnitResponse.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `createUnit`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=createUnit/invocations`
  - **Descripcion:** Inserta una nueva unidad en la tabla DynamoDB.
  - **Operacion DynamoDB:** `PutItem`
    - **Descripcion:** Inserta un nuevo ítem en la tabla DynamoDB.

### 3. getUnitID (Obtener una unidad específica)

- **URL:** `/unidades/{unidadID}/{projectID}`
- **Método HTTP:** GET
- **Descripción:** Recupera la información de una unidad específica por ID en DynamoDB.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `unidadID` (String) Identificador de la unidad
    - `projectID` (String) Identificador del proyecto al cual pertenece la unidad
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplos/getUnitIDResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getUnitID`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getUnitID/invocations`
  - **Descripcion:** Recupera la información de una unidad específica por ID de unidad y proyecto perteneciente.
  - **Operacion DynamoDB:** `GetItem`
    - **Descripcion:** Recupera un ítem específico de la tabla DynamoDB usando `unidadID` como clave de partición y `projectID` como clave de ordenamiento.

### 4. updateUnit (Actualizar una Unidad existente)

- **URL:** `/unidades/{unidadID}`
- **Método HTTP:** PUT
- **Descripción:** Actualiza la información de una unidad existente.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `unidadID` (string) - Identificador de la unidad
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplos/updateUnitRequest.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se confirma mediante un mensaje que fue actualizado unidad indicada al proyecto perteneciente.
    <a href="ejemplos/updateUnitResponse.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `updateUnit`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=updateUnit/invocations`
  - **Descripcion:** Actualiza una unidad existente en la tabla DynamoDB.
  - **Operacion DynamoDB:** `UpdateItem`
    - **Descripcion:** Actualiza un ítem existente en la tabla DynamoDB usando `unidadID` como clave de partición y `projectID` como clave de ordenamiento.

### 5. deleteUnit (Eliminar una unidad de un proyecto en especifico)

- **URL:** `/unidades/{unidadID}/{projectID}`
- **Método HTTP:** DELETE
- **Descripción:** Elimina una unidad existente.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `unidadID` (String) Identificador de la unidad
    - `projectID` (String) Identificador del proyecto al cual pertenece
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplos/deleteUnitResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `deleteUnit`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=deleteUnit/invocations`
  - **Descripcion:** Elimina una unidad que pertenece a un proyecto específico de la tabla DynamoDB.
  - **Operacion DynamoDB:** `DeleteItem`
    - **Descripcion:** Elimina un ítem específico de la tabla DynamoDB usando `unidadID` como clave de partición y `projectID` como clave de ordenamiento.

## Estructura de Datos en DynamoDB
A continuación se muestra la definición de la tabla Unidades en DynamoDB. Esta tabla almacena información sobre Unidades de cada proyecto y está estructurada de la siguiente manera

### Unidad

| Campo | Tipo | Descripcion |
|:-:|:-:|:-:|
|UnidadID|String|Identificador único de la unidad|
|ProyectoID|Map|Identificador del proyecto al que pertenece la unidad.|
|NombreUnidad|String|Nombre de la unidad|
|Precio|Number|Precio de la unidad.|
|Estado|String|Estado actual de la unidad |
|QRCode|String|Código QR asociado a la unidad|
|

### ArrayOfUnit

| Campo | Tipo | Descripcion |
|:-:|:-:|:-:|
|Items|Array|Lista de unidades (Unit).|
|