# Documentación Assesment Pragma 2024

## Introducción

La solución propuesta está diseñada para proporcionar una plataforma integral y eficiente para la gestión de proyectos de construcción y la venta de unidades de vivienda. Aprovechando los servicios avanzados de AWS, la arquitectura está orientada a ofrecer escalabilidad, alta disponibilidad, flexibilidad y seguridad. La plataforma está orientada a mejorar tanto la experiencia del cliente como la eficiencia operativa, permitiendo una gestión efectiva de todos los aspectos del proceso constructivo y comercial.


### Escalabilidad
Capaz de manejar un creciente volumen de datos y usuarios sin afectar el rendimiento.
### Disponibilidad Continua
Con alta disponibilidad y tolerancia a fallos, el sistema está disponible 24/7.
### Flexibilidad
La arquitectura basada en microservicios permite actualizaciones y mejoras rápidas.
### Seguridad
Gestión robusta de usuarios y permisos, asegurando datos sensibles y operaciones seguras.

Esta solución integral combina la potencia y flexibilidad de los servicios de AWS para ofrecer una plataforma robusta y escalable que optimiza la gestión de proyectos de construcción y la venta de unidades de vivienda. La arquitectura basada en microservicios asegura una escalabilidad eficiente, alta disponibilidad, y capacidad para adaptarse rápidamente a las necesidades cambiantes. Además, la seguridad robusta protege los datos y operaciones, mientras que las herramientas de monitoreo aseguran un rendimiento continuo y fiable. Con esta solución, la empresa constructora puede mejorar la experiencia del cliente, aumentar la eficiencia operativa y mantener una ventaja competitiva en el mercado.


#### <a href="collection/AssesmentPRAGMA2024.postman_collection.json" download>Descargar colección Postman</a>
A continuacion se relacionara cada API/servicio que se implemento
## APIs

### [* ProyectosAPI](DOC/Proyectos/Readme.md "ver capacidad")

La API de Proyectos está diseñada para gestionar información relacionada con proyectos de construcción. Permite realizar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) sobre los datos de los proyectos almacenados en un sistema de base de datos.
#### [Ver Swagger](DOC/Proyectos/contrato/API_Proyectos.yaml "ver capacidad")

### [* UnidadesAPI](DOC/Unidades/Readme.md "ver capacidad")

La API de Unidades permite la gestión integral de unidades de vivienda, facilitando la creación, consulta, actualización y eliminación de registros en una tabla DynamoDB dedicada. Esta API es esencial para administrar el inventario de unidades en proyectos residenciales y comerciales, asegurando un manejo eficiente y accesible de la información relevante para cada unidad.
#### [Ver Swagger](DOC/Unidades/contrato/API_Unidades.yaml "ver capacidad")

### [* RecursosAPI](DOC/Recursos/Readme.md "ver capacidad")

La API de Recursos está diseñada para gestionar los recursos asociados a proyectos, como materiales, personal y maquinaria. Permite realizar operaciones CRUD (crear, leer, actualizar y eliminar) sobre los recursos almacenados en DynamoDB, garantizando un control preciso y actualizado del inventario disponible para su asignación en distintos proyectos. 
#### [Ver Swagger](DOC/Recursos/contrato/API_Recursos.yaml "ver capacidad")

### [* VentasReservasAPI](DOC/VentasReservas/Readme.md "ver capacidad")

La API de Ventas y Reservas permite gestionar transacciones relacionadas con la venta y reserva de unidades de vivienda. Facilita la creación, consulta, actualización y eliminación de registros de ventas y reservas en DynamoDB, permitiendo el seguimiento del estado de las transacciones, la asignación de unidades y la gestión de comisiones asociadas a cada operación.
#### [Ver Swagger](DOC/VentasReservas/contrato/API_VentasReservas.yaml "ver capacidad")

### [* ClientesAPI](DOC/Clientes/Readme.md "ver capacidad")

La API de Clientes permite gestionar la información de los clientes en el sistema. Ofrece operaciones para crear, consultar, actualizar y eliminar registros de clientes en DynamoDB. Facilita la administración de datos como nombre, correo electrónico, teléfono, dirección, fecha de nacimiento, documento de identificación y tipo de cliente.
#### [Ver Swagger](DOC/Clientes/contrato/API_Cliente.yaml "ver capacidad")

## Funcionalidad

A continuación se explica la funcionalidad completa de la solución con un diagrama de secuencia:
![Diagrama de secuencia](./diagrams/secuence.svg)

En primer lugar el consumidor realiza la petición al componente de integración pasando en el path el id del cliente. A continuación este componente lanza pediciones ascincronas tanto a customer como a finance_product con el fin de obtener los datos requeridos. Luego se combinan estos dos json en uno solo que sea entendible por el siguiente componente, el cual es el decision_engine. Procesa la lógica necesaria y retorna la misma estructura json pero con los productos fiancieros filtrados.

## Objetos de negocio
![Modelo canónico](./diagrams/canonical.svg)

## Tablas de bases de datos
![Tablas bases de datos](./diagrams/database_tables.svg)

## Modelo de despliegue objetivo

Actualmente el proyecto se encuenta únicamente en local, pero el modelo objetivo de despliegue a futuro es el siguiente:
![Modelo objetivo](./diagrams/target.svg)

En donde se tiene un cluster de EKS en donde deberían estar desplegados todos los componentes en Deploys separados y con una política de HPA. El componente de observabilidad de Prometheus + Grafana también estaría dispuesto dentro de un Deploy independiente.  
Aparte, bases de datos RDS para el almacenamiento de información persistente (customers y financeProducts).  
Un secrets manager para almacenar los secretos de cada uno de los servicios.  
Finalmente una ruta de entrada a través de un Route53, luego por un WAF y finalmente un ALB.

## Diagrama de Componentes y Conectores
![Diagrama componentes y conectores](./diagrams/components_connectors.svg)

En este diagrama se pueden notar las conexiones que realizan los componentes a lo largo de la solución.

## Observabilidad

En la solución implementada, AWS CloudWatch juega un papel fundamental en la observabilidad y el monitoreo continuo del sistema. Proporciona un conjunto completo de herramientas para capturar y analizar métricas clave, así como para realizar un seguimiento en tiempo real del rendimiento de los recursos y las aplicaciones.

CloudWatch permite la creación de dashboards personalizados, donde se visualiza de manera centralizada el progreso del proyecto, el uso de recursos y el rendimiento financiero, proporcionando a los administradores una vista integral de la salud del sistema. Además, la configuración de alarmas y notificaciones automatiza la detección de problemas y la notificación a los equipos responsables, garantizando una respuesta oportuna a cualquier incidente.

## Seguridad

La configuración de seguridad en esta solución se basa en dos componentes clave: la autenticación mediante AWS Cognito y la autorización mediante permisos IAM.

### Autenticación con AWS Cognito
Para proteger el acceso a las APIs desarrolladas, se implementó AWS Cognito como mecanismo de autenticación. Un User Pool de Cognito gestiona las identidades de los usuarios, proporcionando funciones como el registro, inicio de sesión y recuperación de contraseñas. En API Gateway, se configuró un autorizador de tipo Cognito que asocia este User Pool con las diferentes APIs. Esto asegura que solo los usuarios autenticados puedan invocar los métodos de las APIs. Al enviar una solicitud a cualquiera de las APIs, el cliente debe incluir un token JWT (JSON Web Token) válido, que es verificado por el autorizador de Cognito antes de permitir el acceso.

La solución implementada también considera la seguridad general en la interacción entre los servicios de AWS, asegurando que las comunicaciones y accesos entre API Gateway, Lambda, y DynamoDB sean seguros y estén bien controlados.

### Seguridad entre API Gateway y Lambda
La comunicación entre API Gateway y las funciones Lambda se realiza de manera segura mediante el uso de la infraestructura de AWS. API Gateway invoca las funciones Lambda utilizando llamadas internas dentro de la red de AWS, lo que garantiza que el tráfico nunca salga de la infraestructura segura de AWS. Además, API Gateway autentica cada invocación mediante un método seguro, utilizando las credenciales de IAM o roles específicos que limitan el acceso solo a las funciones Lambda necesarias.

### Seguridad entre Lambda y DynamoDB
Las funciones Lambda se comunican con DynamoDB a través de la red interna de AWS, lo que garantiza la seguridad y privacidad de los datos en tránsito. Además, el acceso a DynamoDB está estrictamente controlado mediante roles y políticas IAM. Cada función Lambda tiene un rol IAM asociado que otorga permisos específicos para interactuar con las tablas de DynamoDB. Esto asegura que las funciones Lambda solo puedan realizar las operaciones necesarias (como lectura, escritura, actualización o eliminación de datos) y solo en las tablas para las cuales tienen permisos explícitos.