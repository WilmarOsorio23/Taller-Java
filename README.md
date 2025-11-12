#  Taller Evaluativo: Métodos HTTP

**Materia:** Construcción de Elementos de Software Web III**  
**Modalidad:** Individual  
**Porcentaje:** 30% del Taller 1  
**Fecha de entrega:** Hasta el 12 de noviembre  
**Estudiante:** Wilmar Osorio  

---

##  Objetivo de la Actividad

El propósito de este taller es comprender y aplicar los **métodos HTTP** dentro del desarrollo de software web.  
Se busca que el estudiante identifique los diferentes métodos, su aplicabilidad, su relación con la arquitectura web (REST, SOAP, etc.) y su implementación práctica mediante ejemplos y código real.

---

##  1. Listado de Métodos HTTP

| Método | Descripción |
|:--------|:-------------|
| **GET** | Solicita datos de un recurso en el servidor. |
| **POST** | Envía datos al servidor para crear un nuevo recurso. |
| **PUT** | Reemplaza completamente un recurso existente. |
| **DELETE** | Elimina un recurso del servidor. |
| **PATCH** | Modifica parcialmente un recurso existente. |
| **OPTIONS** | Muestra los métodos disponibles para un recurso. |
| **HEAD** | Devuelve solo los encabezados, sin el cuerpo de la respuesta. |
| **CONNECT** | Crea un túnel entre cliente y servidor, usado para HTTPS. |
| **TRACE** | Retorna la solicitud original para fines de depuración. |

---

##  2. Aplicabilidad de Cada Método

| Método | Cuándo se usa | Por qué se usa |
|:--------|:----------------|:----------------|
| **GET** | Para **consultar información** o leer datos del servidor. | Es un método seguro e idempotente. No altera el estado del servidor. |
| **POST** | Para **enviar datos** al servidor y **crear recursos nuevos**. | Permite enviar datos en el cuerpo (body) de la solicitud. |
| **PUT** | Para **actualizar completamente** un recurso existente. | Asegura que el recurso se reemplace por completo. |
| **PATCH** | Para **modificar parcialmente** un recurso existente. | Optimiza el uso de datos, modificando solo los campos necesarios. |
| **DELETE** | Para **eliminar recursos** del servidor. | Mantiene el control sobre los registros o entidades. |
| **OPTIONS** | Para **conocer los métodos permitidos** por un recurso. | Se usa en validaciones de **CORS**. |
| **HEAD** | Para **verificar encabezados** (tipo, longitud, fecha, etc.). | Reduce el consumo de red. |
| **CONNECT** | Para establecer una **conexión segura**. | Es base del protocolo **HTTPS**. |
| **TRACE** | Para **verificar solicitudes** y depurar errores. | Permite conocer la solicitud exacta enviada. |

---

##  3. Relación con la Arquitectura Web

###  Arquitectura REST (Representational State Transfer)

REST utiliza los métodos HTTP para definir acciones sobre recursos, cumpliendo el principio **CRUD**:

| Acción | Método HTTP | Descripción |
|:--------|:---------------|:-------------|
| **Create** | POST | Crea un nuevo recurso. |
| **Read** | GET | Consulta uno o varios recursos. |
| **Update** | PUT / PATCH | Modifica un recurso existente. |
| **Delete** | DELETE | Elimina un recurso. |

REST se basa en **URI (Uniform Resource Identifier)**, usa **JSON o XML** como formato de intercambio y se caracteriza por su **simplicidad y escalabilidad**.

---

###  Arquitectura SOAP (Simple Object Access Protocol)

SOAP también usa HTTP, pero no depende de sus métodos.  
Normalmente utiliza **POST** para enviar mensajes XML estructurados que contienen la información de la operación.  
A diferencia de REST, SOAP se enfoca más en la **formalidad, seguridad y confiabilidad de los mensajes**, siendo común en entornos empresariales.

---

###  Otras Arquitecturas Modernas

- **GraphQL:** usa principalmente **POST**, ya que las consultas se envían en el cuerpo de la solicitud.  
- **gRPC:** emplea **HTTP/2** y define métodos mediante archivos `.proto`, lo que lo hace muy eficiente en microservicios.

---

##  4. Forma de Uso: Ejemplos Prácticos y Sintaxis

A continuación se presentan ejemplos prácticos de uso de los principales métodos HTTP y una implementación completa en Python con Flask.

---

###  Ejemplos de Peticiones HTTP

```http
### Ejemplo 1: Método GET
GET /api/usuarios HTTP/1.1
Host: ejemplo.com
Accept: application/json
Authorization: Bearer <token>

### Ejemplo 2: Método POST
POST /api/usuarios HTTP/1.1
Host: ejemplo.com
Content-Type: application/json

{
  "nombre": "Wilmar Osorio",
  "correo": "wilmar@example.com",
  "rol": "estudiante"
}

### Ejemplo 3: Método PUT
PUT /api/usuarios/1 HTTP/1.1
Host: ejemplo.com
Content-Type: application/json

{
  "nombre": "Wilmar A. Osorio",
  "correo": "wilmar@example.com",
  "rol": "developer"
}

### Ejemplo 4: Método PATCH
PATCH /api/usuarios/1 HTTP/1.1
Host: ejemplo.com
Content-Type: application/json

{
  "rol": "administrador"
}

### Ejemplo 5: Método DELETE
DELETE /api/usuarios/1 HTTP/1.1
Host: ejemplo.com
Authorization: Bearer <token>
