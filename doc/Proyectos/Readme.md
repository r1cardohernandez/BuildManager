# Documentación de la API de Proyectos

## Introducción

La **API de Proyectos** permite gestionar proyectos de construcción almacenados en DynamoDB. La API ofrece funcionalidades para obtener, crear, actualizar y eliminar proyectos. Está diseñada para integrarse con Lambda y utiliza AWS Cognito para autenticación.

* [Ver Swagger](contrato/API_Proyectos.yaml "ver capacidad")

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

- **Base URL:** `https://8her9az8a3.execute-api.us-east-2.amazonaws.com/{basePath}`
  - **Variable de URL:** `basePath` (default: `DEV`)

## Endpoints

### 1. getProjects (Obtener Todos los Proyectos)

- **URL:** `/proyectos`
- **Método HTTP:** GET
- **Descripción:** Recupera una lista de todos los proyectos almacenados en DynamoDB.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/getProjectsResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getProjects`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getProjects/invocations`
  - **Descripcion:** Recupera todos los proyectos desde la tabla DynamoDB.
  - **Operacion DynamoDB:** `Scan`
    - **Descripcion:** Escanea la tabla DynamoDB y devuelve todos los ítems. Se puede aplicar un filtro si es necesario.

### 2. createProject (Crear un Nuevo Proyecto)

- **URL:** `/proyectos`
- **Método HTTP:** POST
- **Descripción:** Crea un nuevo proyecto de construcción.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/createProjectRequest&Response.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se muestra el mismo body tanto en el request como en el response validadno la inforamcion subida.
    <a href="ejemplo/createProjectRequest&Response.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `createProjects`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:createProjects/invocations`
  - **Descripcion:** Inserta un nuevo proyecto en la tabla DynamoDB.
  - **Operacion DynamoDB:** `PutItem`
    - **Descripcion:** Inserta un nuevo ítem en la tabla DynamoDB. Si el `ProjectID` ya existe, la operación puede ser configurada para reemplazar el ítem existente.

### 3. getProjectID (Obtener un Proyecto Específico)

- **URL:** `/proyectos/{projectID}`
- **Método HTTP:** GET
- **Descripción:** Recupera la información de un proyecto específico por ID en DynamoDB.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `projectID` (string) - ID del proyecto a consultar
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/getProjectIDResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getProjectID`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getProjectID/invocations`
  - **Descripcion:** Recupera la información de un proyecto específico por ID.
  - **Operacion DynamoDB:** `GetItem`
    - **Descripcion:** Recupera un ítem específico de la tabla DynamoDB usando ProjectID como clave de partición.

### 4. updateProject (Actualizar un Proyecto)

- **URL:** `/proyectos/{projectID}`
- **Método HTTP:** PUT
- **Descripción:** Actualiza la información de un proyecto existente.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `projectID` (string) - ID del proyecto a actualizar
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/updateProjectRequest.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se confirma mediante un mensaje que fue actualizado el proyecto indicado.
    <a href="ejemplo/updateProjectResponse.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `updateProjects`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:updateProjects/invocations`
  - **Descripcion:** Actualiza un proyecto existente en la tabla DynamoDB.
  - **Operacion DynamoDB:** `UpdateItem`
    - **Descripcion:** Actualiza un ítem existente en la tabla DynamoDB usando ProjectID como clave de partición.

### 5. deleteProject (Eliminar un Proyecto)

- **URL:** `/proyectos/{projectID}`
- **Método HTTP:** DELETE
- **Descripción:** Elimina un proyecto existente.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `projectID` (string) - ID del proyecto a eliminar.
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/deleteProjectResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `deleteProject`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:deleteProject/invocations`
  - **Descripcion:** Elimina un proyecto específico de la tabla DynamoDB.
  - **Operacion DynamoDB:** `DeleteItem`
    - **Descripcion:** Elimina un ítem específico de la tabla DynamoDB usando ProjectID como clave de partición.

## Estructura de Datos en DynamoDB
A continuación se muestra la definición de la tabla Proyectos en DynamoDB. Esta tabla almacena información sobre proyectos de construcción y está estructurada de la siguiente manera

| Campo | Tipo | Descripcion |
|:-:|:-:|:-:|
|ProjectID|String|Identificador único del proyecto|
|AssignedResources|Map|Recursos asignados al proyecto. Incluye:|
|||- Machinery: Descripción de la maquinaria asignada.|
|||- Materials: Descripción de los materiales asignados.|
|||- Personnel: Descripción del personal asignado.|
|EndDate|String|Fecha de finalización del proyecto en formato  `YYYY-MM-DD`.|
|ProjectName|String|Nombre del proyecto|
|ProjectType|String|Tipo de proyecto |
|Responsible|String|Persona responsable del proyecto|
|Stage|String|Etapa actual del proyecto|
|StartDate|String|Fecha de inicio del proyecto en formato `YYYY-MM-DD`.|
|Type|String|Tipo de entidad |
|


