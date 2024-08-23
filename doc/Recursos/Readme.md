# Documentación de la API de Recursos

## Introducción

La **API de Recursos** permite gestionar recursos en la base de datos. Ofrece funcionalidades para obtener, crear, actualizar y eliminar recursos. Está diseñada para integrarse con Lambda y utiliza autenticación mediante tokens JWT.

* [Ver Swagger](contrato/API_Recursos.yaml "ver capacidad")

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

- **Base URL:** `https://tera890pij.execute-api.us-east-2.amazonaws.com/{basePath}`
  - **Variable de URL:** `basePath` (default: `dev`)

## Endpoints

### 1. getResources (Obtener Todos los Recursos)

- **URL:** `/recursos`
- **Método HTTP:** GET
- **Descripción:** Recupera una lista de todos los recursos almacenados.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/getResourcesResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getProjects`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getResources/invocations`
  - **Descripcion:** Recupera todos los recursos desde la base de datos.
  - **Operacion DynamoDB:** `Scan`
    - **Descripcion:** Escanea la tabla y devuelve todos los ítems. Se puede aplicar un filtro si es necesario.

### 2. createResource (Crear un Nuevo Recurso)

- **URL:** `/recursos`
- **Método HTTP:** POST
- **Descripción:** Crea un nuevo recurso
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/createProjectRequest&Response.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se muestra el mismo body tanto en el request como en el response validadno la inforamcion subida.
    <a href="ejemplo/createResourceRequest&Response.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `createResources`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:createResources/invocations`
  - **Descripcion:** Inserta un nuevo recurso en la base de datos.
  - **Operacion DynamoDB:** `PutItem`
    - **Descripcion:** Inserta un nuevo ítem en la base de datos.

### 3. getResourceID (Obtener un Recurso Específico)

- **URL:** `/recursos/{recursoID}`
- **Método HTTP:** GET
- **Descripción:** Recupera la información de un recurso específico por ID.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `recursoID` (string) - ID del recurso a consultar.
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/getResourceIDResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getResourcesID`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getResourcesID/invocations`
  - **Descripcion:** Recupera la información de un recurso específico por ID.
  - **Operacion DynamoDB:** `GetItem`
    - **Descripcion:** Recupera un ítem específico de la base de datos usando RecursoID como clave de partición.

### 4. updateResources (Actualizar un Recurso)

- **URL:** `/recursos/{recursoID}`
- **Método HTTP:** PUT
- **Descripción:** Actualiza la información de un recurso existente.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `recursoID` (string) - ID del recurso a consultar.
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/updateResourceRequest.json" download>Descargar json bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se confirma mediante un body que fue actualizado el recurso indicado.
    <a href="ejemplo/updateResourceResponse.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `updateProjects`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:updateResources/invocations`
  - **Descripcion:** Actualiza un recurso existente en la base de datos.
  - **Operacion DynamoDB:** `UpdateItem`
    - **Descripcion:** Actualiza un ítem existente en la base de datos usando RecursoID como clave de partición.

### 5. deleteResource (Eliminar un Recurso)

- **URL:** `/recursos/{recursoID}`
- **Método HTTP:** DELETE
- **Descripción:** Elimina un recurso existente.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `recursoID` (string) - ID del recurso a consultar.
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 204 No content
  - **Formato de Respuesta:** No hay formato de respuesta pues un status 204 indica que la operacion fue exitosa sin mensaje o body de respuesta
- **Funcion lambda**  `deleteResources`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:deleteResources/invocations`
  - **Descripcion:** Elimina un recurso específico de la base de datos.
  - **Operacion DynamoDB:** `DeleteItem`
    - **Descripcion:** Elimina un ítem específico de la base de datos usando RecursoID como clave de partición.

## Estructura de Datos en DynamoDB
A continuación se muestra la definición de la tabla Recursos en DynamoDB. Esta tabla almacena información sobre recursos y está estructurada de la siguiente manera:

| Campo | Tipo | Descripcion |
|:-:|:-:|:-:|
|RecursoID|String|Identificador único del recurso|
|NombreRecurso|String|Nombre del recurso|
|CantidadDisponible|Integer|Cantidad disponible del recurso|
|TipoRecurso|String|Tipo de recurso (e.g., "Material", "Herramienta") |
|


