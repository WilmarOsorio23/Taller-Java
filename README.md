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



###  Código completo: `app.py`
```python
# ==========================================
#  API Básica con Métodos HTTP en Flask
# Autor: Wilmar Osorio
# ==========================================

from flask import Flask, jsonify, request

app = Flask(__name__)

# Base de datos temporal (solo para ejemplo)
usuarios = [
    {"id": 1, "nombre": "Carlos"},
    {"id": 2, "nombre": "María"}
]

# ==========================================
#  MÉTODO GET - Consultar información
# ==========================================
@app.route('/usuarios', methods=['GET'])
def obtener_usuarios():
    # Devuelve la lista completa de usuarios.
    # Ejemplo: GET http://localhost:5000/usuarios
    return jsonify(usuarios), 200

@app.route('/usuarios/<int:id>', methods=['GET'])
def obtener_usuario(id):
    # Devuelve un usuario específico según su ID.
    # Ejemplo: GET http://localhost:5000/usuarios/1
    for usuario in usuarios:
        if usuario["id"] == id:
            return jsonify(usuario), 200
    return jsonify({"mensaje": "Usuario no encontrado"}), 404

# ==========================================
#  MÉTODO POST - Crear nuevo usuario
# ==========================================
@app.route('/usuarios', methods=['POST'])
def crear_usuario():
    # Crea un nuevo usuario.
    # Ejemplo: POST http://localhost:5000/usuarios
    # JSON:
    # { "nombre": "Laura" }
    nuevo_usuario = request.get_json()
    nuevo_usuario["id"] = len(usuarios) + 1
    usuarios.append(nuevo_usuario)
    return jsonify({"mensaje": "Usuario agregado correctamente", "data": nuevo_usuario}), 201

# ==========================================
#  MÉTODO PUT - Actualizar usuario existente
# ==========================================
@app.route('/usuarios/<int:id>', methods=['PUT'])
def actualizar_usuario(id):
    # Actualiza el nombre de un usuario existente.
    # Ejemplo: PUT http://localhost:5000/usuarios/1
    # JSON:
    # { "nombre": "Carlos Actualizado" }
    datos = request.get_json()
    for usuario in usuarios:
        if usuario["id"] == id:
            usuario["nombre"] = datos.get("nombre", usuario["nombre"])
            return jsonify({"mensaje": "Usuario actualizado correctamente", "data": usuario}), 200
    return jsonify({"mensaje": "Usuario no encontrado"}), 404

# ==========================================
#  MÉTODO DELETE - Eliminar usuario
# ==========================================
@app.route('/usuarios/<int:id>', methods=['DELETE'])
def eliminar_usuario(id):
    # Elimina un usuario de la lista.
    # Ejemplo: DELETE http://localhost:5000/usuarios/2
    for usuario in usuarios:
        if usuario["id"] == id:
            usuarios.remove(usuario)
            return jsonify({"mensaje": "Usuario eliminado correctamente"}), 200
    return jsonify({"mensaje": "Usuario no encontrado"}), 404

# ==========================================
#  Ejecutar la app
# ==========================================
if __name__ == '__main__':
    app.run(debug=True)
```

---

##  Cómo probar la API

1. Guarda el archivo como `app.py`
2. Instala Flask:
   ```bash
   pip install flask
   ```
3. Ejecuta el servidor:
   ```bash
   python app.py
   ```
4. Abre **Postman** o tu navegador y prueba:

| Método | URL | Descripción |
|---------|-----|-------------|
| `GET` | http://localhost:5000/usuarios | Muestra todos los usuarios |
| `GET` | http://localhost:5000/usuarios/1 | Muestra un usuario específico |
| `POST` | http://localhost:5000/usuarios | Crea un nuevo usuario |
| `PUT` | http://localhost:5000/usuarios/1 | Actualiza un usuario |
| `DELETE` | http://localhost:5000/usuarios/2 | Elimina un usuario |

---

##  Ejemplo de uso en Postman

###  POST
- URL: `http://localhost:5000/usuarios`
- Body (JSON):
```json
{
  "nombre": "Laura"
}
```

###  PUT
- URL: `http://localhost:5000/usuarios/1`
- Body (JSON):
```json
{
  "nombre": "Carlos Actualizado"
}
```

###  DELETE
- URL: `http://localhost:5000/usuarios/2`

---

##  Resumen final

| Método | Acción | Ejemplo simple | Descripción |
|---------|---------|----------------|--------------|
| **GET** | Leer | Ver todos los usuarios | Solo consulta datos |
| **POST** | Crear | Agregar un usuario nuevo | Envía datos nuevos |
| **PUT** | Actualizar | Cambiar nombre de un usuario | Modifica datos existentes |
| **DELETE** | Eliminar | Borrar un usuario | Quita datos del servidor |

---

