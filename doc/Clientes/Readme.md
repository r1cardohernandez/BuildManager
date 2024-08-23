# Documentación de la API de Clientes

## Introducción

La **API Clientes** permite la gestión de registros de clientes, incluyendo la creación, recuperación, actualización y eliminación de datos. La API está configurada para interactuar con AWS Lambda para realizar estas operaciones.

- [Ver Swagger](contrato/API_Cliente.yaml "ver capacidad")

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

- **Base URL:** `https://n0lolxi73d.execute-api.us-east-2.amazonaws.com/{basePath}`
  - **Variable de URL:** `basePath` (default: `DEV`)

## Endpoints

### 1. getCustomer (Listar Clientes)

- **URL:** `/clientes`
- **Método HTTP:** GET
- **Descripción:** Recupera una lista de todos los clientes.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  ```json
  [
    {
        "DocumentoID": "1234567891",
        "Telefono": "+1234567891",
        "Nombre": "Luis Pérez",
        "TipoCliente": "Corporativo",
        "ClienteID": "C002",
        "CorreoElectronico": "luis.perez@example.com",
        "Direccion": "Avenida Siempre Viva 742, Ciudad, País",
        "FechaNacimiento": "1975-08-22"
    },
    {
        "DocumentoID": "1234567894",
        "Telefono": "+1234567894",
        "Nombre": "Sofía López",
        "TipoCliente": "Particular",
        "ClienteID": "C005",
        "CorreoElectronico": "sofia.lopez@example.com",
        "Direccion": "Calle de la Amargura 321, Ciudad, País",
        "FechaNacimiento": "1995-07-21"
    },
    {
        "DocumentoID": "1234567801",
        "Telefono": "+1234567801",
        "Nombre": "Fernando Vega",
        "TipoCliente": "Corporativo",
        "ClienteID": "C012",
        "CorreoElectronico": "fernando.vega@example.com",
        "Direccion": "Avenida del Sol 789, Ciudad, País",
        "FechaNacimiento": "1979-10-09"
    }
  ]
  ```
- **Funcion lambda**  `getCustomer`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function:getCustomer/invocations`
  - **Descripcion:** Recupera todos los clientes desde la base de datos
  - **Operacion DynamoDB:** `Scan`
    - **Descripcion:** Escanea la tabla DynamoDB y devuelve todos los ítems. Se puede aplicar un filtro si es necesario.

### 2. createCustomer (Crear Cliente)

- **URL:** `/clientes`
- **Método HTTP:** POST
- **Descripción:** Crea un nuevo cliente.
- **Parámetros de Solicitud:**
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/createProjectRequest&Response.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se muestra el mismo body tanto en el request como en el response validadno la inforamcion subida.
    <a href="ejemplo/createProjectRequest&Response.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `createCustomer`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=createCustomer/invocations`
  - **Descripcion:** Crea un nuevo cliente en la base de datos.
  - **Operacion DynamoDB:** `PutItem`
    - **Descripcion:** Inserta un nuevo ítem en la tabla DynamoDB.

### 3. getCustomerID (Obtener Cliente por ID)

- **URL:** `/clientes/{clienteID}`
- **Método HTTP:** GET
- **Descripción:** Recupera la información de un cliente específico usando su ID.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `clienteID ` (string) - ID del cliente a consultar
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** <a href="ejemplo/getProjectIDResponse.json" download>Descargar json respuesta</a>
- **Funcion lambda**  `getCustomerID`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=getCustomerID/invocations`
  - **Descripcion:** Recupera la información de un cliente específico desde la base de datos.
  - **Operacion DynamoDB:** `GetItem`
    - **Descripcion:** Recupera un ítem específico de una tabla DynamoDB utilizando la clave primaria `clienteID`.

### 4. updateCustomer (Actualizar Cliente)

- **URL:** `/clientes/{clienteID}`
- **Método HTTP:** PUT
- **Descripción:** Actualiza la información de un cliente existente.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `clienteID ` (string) - ID del cliente a consultar
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Request & Response:**
  -**Body:** <a href="ejemplo/updateCustomerRequest.json" download>Descargar josn bodyRequest</a>
  - **Codigo de estado:** 200 ok
  - **Formato de Respuesta:** Se confirma mediante un mensaje que fue actualizado el proyecto indicado.
    <a href="ejemplo/updateCustomerResponse.json" download>Descargar josn respuesta</a>
- **Funcion lambda**  `updateCustomer`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=updateCustomer/invocations`
  - **Descripcion:** Actualiza la información de un cliente existente en la base de datos.
  - **Operacion DynamoDB:** `UpdateItem`
    - **Descripcion:**  Actualiza un ítem en una tabla DynamoDB utilizando la clave primaria y los atributos proporcionados.

### 5. deleteCustomer (Eliminar Cliente)

- **URL:** `/clientes/{clienteID}`
- **Método HTTP:** DELETE
- **Descripción:** Elimina un cliente específico.
- **Parámetros de Solicitud:**
  - **Path Parametro:**
    - `clienteID ` (string) - ID del cliente a consultar
  - **Header:**
    - `Authorization` (string) - Token JWT para autenticación.
- **Respuesta:**
  - **Codigo de estado:** 204 No Content
  - **Formato de Respuesta:** no existe formato de respuesta debido al tratarse de un status 204 que confirma como satisfactoria la peticion pero sin contenido de respuesta
- **Funcion lambda**  `deleteCustomer`
  - **ARN:** `arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:941377161031:function=deleteCustomer/invocations`
  - **Descripcion:** Elimina un cliente específico de la base de datos.
  - **Operacion DynamoDB:** `DeleteItem`
    - **Descripcion:** Elimina un ítem de una tabla DynamoDB utilizando la clave primaria.

## Estructura de Datos en DynamoDB
A continuación se muestra la definición de la tabla Clientes en DynamoDB. Esta tabla almacena información sobre clientes y está estructurada de la siguiente manera:

| Campo | Tipo | Descripcion |
|:-:|:-:|:-:|
|ClienteID|String|Identificador único del cliente.|
|CorreoElectronico|String|Correo electrónico del cliente.|
|Direccion|String|Dirección del cliente.|
|DocumentoID|String|Identificación del documento del cliente.
|FechaNacimiento|String|Fecha de nacimiento del cliente en formato `YYYY-MM-DD`. |
|Nombre|String|Nombre del cliente.|
|Telefono|String|Número de teléfono del cliente.
|TipoCliente|String|Tipo de Cliente|
|


