# API REST - Plataforma Freelance

## Información General

**Base URL:** `https://api.freelanceplatform.com`

**Versión:** v1

**Autenticación:** JWT Bearer Token

## Códigos de Estado HTTP

| Código | Descripción |
|--------|-------------|
| 200 | OK - Solicitud exitosa |
| 201 | Created - Recurso creado exitosamente |
| 400 | Bad Request - Error en la solicitud |
| 401 | Unauthorized - Token no válido o faltante |
| 403 | Forbidden - Sin permisos suficientes |
| 404 | Not Found - Recurso no encontrado |
| 422 | Unprocessable Entity - Error de validación |
| 500 | Internal Server Error - Error del servidor |

## Estructura de respuesta

Todas las respuestas siguen este formato:

```json
{
  "success": true,
  "data": {
    // Datos de la respuesta
  },
  "error": {
    "message": "Mensaje de error",
    "code": "ERROR_CODE"
  }
}
```

---

## 🔐 Autenticación

### Registro de Usuario

**POST** `/api/auth/register`

Registra un nuevo usuario en la plataforma.

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| email | string | ✅ | Email del usuario |
| password | string | ✅ | Contraseña (mín. 6 caracteres) |
| first_name | string | ✅ | Nombre |
| last_name | string | ✅ | Apellido |
| phone | string | ❌ | Teléfono |
| user_type | string | ✅ | Tipo: `freelancer` o `client` |

**Ejemplo de solicitud:**

```json
{
  "email": "user@example.com",
  "password": "password123",
  "first_name": "Juan",
  "last_name": "Pérez",
  "phone": "+56912345678",
  "user_type": "freelancer"
}
```

**Respuesta exitosa (201):**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": 1,
      "email": "user@example.com",
      "first_name": "Juan",
      "last_name": "Pérez",
      "user_type": "freelancer"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

### Iniciar Sesión

**POST** `/api/auth/login`

Autentica un usuario existente.

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| email | string | ✅ | Email del usuario |
| password | string | ✅ | Contraseña |

**Ejemplo de solicitud:**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": 1,
      "email": "user@example.com",
      "first_name": "Juan",
      "last_name": "Pérez",
      "user_type": "freelancer"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

## 📋 Proyectos

### Listar Proyectos

**GET** `/api/projects`

Obtiene una lista paginada de proyectos disponibles.

**Parámetros de consulta:**

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| page | number | Número de página (default: 1) |
| limit | number | Elementos por página (default: 10) |
| category | number | ID de categoría |
| budget_min | number | Presupuesto mínimo |
| budget_max | number | Presupuesto máximo |

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "projects": [
      {
        "id": 1,
        "title": "Desarrollo de E-commerce",
        "description": "Necesitamos desarrollar una tienda online completa...",
        "budget_min": 500,
        "budget_max": 1000,
        "deadline": "2024-04-15",
        "status": "open",
        "category": {
          "id": 1,
          "name": "Desarrollo"
        },
        "client": {
          "id": 2,
          "first_name": "María",
          "last_name": "González",
          "avatar_url": "https://example.com/avatar.jpg",
          "rating": 4.7
        },
        "skills_required": [
          {
            "id": 1,
            "name": "React"
          },
          {
            "id": 2,
            "name": "Node.js"
          }
        ],
        "proposals_count": 8,
        "created_at": "2024-03-15T08:00:00Z"
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 47
    }
  }
}
```

### Crear Proyecto

**POST** `/api/projects`

Crea un nuevo proyecto. Requiere autenticación como cliente.

**Headers:**
```
Authorization: Bearer {token}
```

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| title | string | ✅ | Título del proyecto |
| description | string | ✅ | Descripción detallada |
| category_id | number | ✅ | ID de la categoría |
| budget_min | number | ✅ | Presupuesto mínimo |
| budget_max | number | ✅ | Presupuesto máximo |
| deadline | string | ✅ | Fecha límite (YYYY-MM-DD) |
| skills_required | array | ✅ | Array de IDs de habilidades |
| attachments | array | ❌ | Archivos adjuntos |

**Ejemplo de solicitud:**

```json
{
  "title": "Desarrollo de E-commerce",
  "description": "Necesitamos desarrollar una tienda online completa...",
  "category_id": 1,
  "budget_min": 500,
  "budget_max": 1000,
  "deadline": "2024-04-15",
  "skills_required": [1, 2, 3],
  "attachments": [
    {
      "file_name": "brief.pdf",
      "file_data": "base64_encoded_file"
    }
  ]
}
```

**Respuesta exitosa (201):**

```json
{
  "success": true,
  "data": {
    "project": {
      "id": 1,
      "title": "Desarrollo de E-commerce",
      "description": "Necesitamos desarrollar una tienda online completa...",
      "budget_min": 500,
      "budget_max": 1000,
      "deadline": "2024-04-15",
      "status": "open",
      "created_at": "2024-03-15T10:30:00Z"
    }
  }
}
```

### Obtener Proyecto por ID

**GET** `/api/projects/{id}`

Obtiene los detalles completos de un proyecto específico.

**Parámetros de ruta:**

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | number | ID del proyecto |

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "project": {
      "id": 1,
      "title": "Desarrollo de E-commerce",
      "description": "Necesitamos desarrollar una tienda online completa...",
      "budget_min": 500,
      "budget_max": 1000,
      "deadline": "2024-04-15",
      "status": "open",
      "client": {
        "id": 2,
        "first_name": "María",
        "last_name": "González",
        "avatar_url": "https://example.com/avatar.jpg",
        "bio": "Fundadora de StartupTech...",
        "location": "Santiago, Chile",
        "rating": 4.7,
        "projects_completed": 12,
        "is_verified": true
      },
      "category": {
        "id": 1,
        "name": "Desarrollo"
      },
      "skills_required": [
        {
          "id": 1,
          "name": "React"
        }
      ],
      "attachments": [
        {
          "id": 1,
          "file_name": "brief.pdf",
          "file_url": "https://example.com/files/brief.pdf"
        }
      ],
      "proposals": [
        {
          "id": 1,
          "freelancer": {
            "first_name": "Juan",
            "last_name": "Developer",
            "avatar_url": "https://example.com/avatar.jpg",
            "rating": 4.9
          },
          "price": 750,
          "delivery_time": 15,
          "cover_letter": "Tengo 5 años de experiencia...",
          "status": "pending",
          "created_at": "2024-03-15T12:00:00Z"
        }
      ],
      "proposals_count": 8,
      "created_at": "2024-03-15T08:00:00Z"
    }
  }
}
```

---

## 📝 Propuestas

### Crear Propuesta

**POST** `/api/proposals`

Crea una nueva propuesta para un proyecto. Requiere autenticación como freelancer.

**Headers:**
```
Authorization: Bearer {token}
```

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| project_id | number | ✅ | ID del proyecto |
| price | number | ✅ | Precio propuesto |
| delivery_time | number | ✅ | Tiempo de entrega en días |
| cover_letter | string | ✅ | Carta de presentación |

**Ejemplo de solicitud:**

```json
{
  "project_id": 1,
  "price": 750,
  "delivery_time": 15,
  "cover_letter": "Estimado cliente, tengo 5 años de experiencia desarrollando e-commerce..."
}
```

**Respuesta exitosa (201):**

```json
{
  "success": true,
  "data": {
    "proposal": {
      "id": 1,
      "project_id": 1,
      "freelancer_id": 1,
      "price": 750,
      "delivery_time": 15,
      "cover_letter": "Estimado cliente, tengo 5 años de experiencia...",
      "status": "pending",
      "created_at": "2024-03-15T12:00:00Z"
    }
  }
}
```

### Cambiar Estado de Propuesta

**PUT** `/api/proposals/{id}/status`

Cambia el estado de una propuesta. Requiere autenticación como cliente propietario del proyecto.

**Headers:**
```
Authorization: Bearer {token}
```

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| status | string | ✅ | Estado: `accepted` o `rejected` |

**Ejemplo de solicitud:**

```json
{
  "status": "accepted"
}
```

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "proposal": {
      "id": 1,
      "status": "accepted",
      "updated_at": "2024-03-15T14:00:00Z"
    }
  }
}
```

---

## 🛍️ Servicios

### Listar Servicios

**GET** `/api/services`

Obtiene una lista paginada de servicios disponibles.

**Parámetros de consulta:**

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| page | number | Número de página (default: 1) |
| limit | number | Elementos por página (default: 10) |
| category | number | ID de categoría |
| min_price | number | Precio mínimo |
| max_price | number | Precio máximo |
| search | string | Búsqueda por título |

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "services": [
      {
        "id": 1,
        "title": "Desarrollo Web Full Stack",
        "description": "Desarrollo completo de aplicaciones web...",
        "price": 150,
        "delivery_time": 7,
        "category": {
          "id": 1,
          "name": "Desarrollo"
        },
        "user": {
          "id": 1,
          "first_name": "Juan",
          "last_name": "Pérez",
          "avatar_url": "https://example.com/avatar.jpg"
        },
        "main_image": "https://example.com/service-image.jpg",
        "rating": 4.9,
        "reviews_count": 45
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 10,
      "total_items": 95
    }
  }
}
```

### Crear Servicio

**POST** `/api/services`

Crea un nuevo servicio. Requiere autenticación.

**Headers:**
```
Authorization: Bearer {token}
```

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| title | string | ✅ | Título del servicio |
| description | string | ✅ | Descripción detallada |
| price | number | ✅ | Precio del servicio |
| delivery_time | number | ✅ | Tiempo de entrega en días |
| category_id | number | ✅ | ID de la categoría |
| skills | array | ✅ | Array de IDs de habilidades |
| images | array | ❌ | Imágenes en base64 |

**Ejemplo de solicitud:**

```json
{
  "title": "Desarrollo Web Full Stack",
  "description": "Desarrollo completo de aplicaciones web...",
  "price": 150,
  "delivery_time": 7,
  "category_id": 1,
  "skills": [1, 2, 3],
  "images": ["base64_image_1", "base64_image_2"]
}
```

**Respuesta exitosa (201):**

```json
{
  "success": true,
  "data": {
    "service": {
      "id": 1,
      "title": "Desarrollo Web Full Stack",
      "description": "Desarrollo completo de aplicaciones web...",
      "price": 150,
      "delivery_time": 7,
      "status": "active",
      "created_at": "2024-03-15T10:30:00Z"
    }
  }
}
```

### Obtener Servicio por ID

**GET** `/api/services/{id}`

Obtiene los detalles completos de un servicio específico.

**Parámetros de ruta:**

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | number | ID del servicio |

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "service": {
      "id": 1,
      "title": "Desarrollo Web Full Stack",
      "description": "Desarrollo completo de aplicaciones web...",
      "price": 150,
      "delivery_time": 7,
      "user": {
        "id": 1,
        "first_name": "Juan",
        "last_name": "Pérez",
        "avatar_url": "https://example.com/avatar.jpg",
        "bio": "Desarrollador con 5 años de experiencia...",
        "location": "Santiago, Chile",
        "rating": 4.9
      },
      "category": {
        "id": 1,
        "name": "Desarrollo"
      },
      "images": [
        {
          "id": 1,
          "url": "https://example.com/image1.jpg",
          "is_main": true
        }
      ],
      "skills": [
        {
          "id": 1,
          "name": "React"
        }
      ],
      "reviews": [
        {
          "id": 1,
          "rating": 5,
          "comment": "Excelente trabajo",
          "user": {
            "first_name": "María",
            "last_name": "García"
          },
          "created_at": "2024-03-10T15:20:00Z"
        }
      ]
    }
  }
}
```

---

## 👤 Usuario

### Obtener Perfil

**GET** `/api/users/profile`

Obtiene el perfil del usuario autenticado.

**Headers:**
```
Authorization: Bearer {token}
```

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": 1,
      "email": "user@example.com",
      "first_name": "Juan",
      "last_name": "Pérez",
      "phone": "+56912345678",
      "user_type": "freelancer",
      "avatar_url": "https://example.com/avatar.jpg",
      "bio": "Desarrollador Full Stack con 5 años de experiencia...",
      "location": "Santiago, Chile",
      "is_verified": true,
      "skills": [
        {
          "id": 1,
          "name": "React",
          "level": "expert"
        }
      ],
      "services_count": 5,
      "rating": 4.9,
      "reviews_count": 23
    }
  }
}
```

---

## 📁 Categorías

### Listar Categorías

**GET** `/api/categories`

Obtiene todas las categorías disponibles.

**Respuesta exitosa (200):**

```json
{
  "success": true,
  "data": {
    "categories": [
      {
        "id": 1,
        "name": "Desarrollo",
        "description": "Servicios de desarrollo web y móvil",
        "services_count": 156
      },
      {
        "id": 2,
        "name": "Diseño",
        "description": "Servicios de diseño gráfico y UX/UI",
        "services_count": 89
      }
    ]
  }
}
```

---

## 📦 Órdenes

### Crear Orden

**POST** `/api/orders`

Crea una nueva orden para comprar un servicio.

**Headers:**
```
Authorization: Bearer {token}
```

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| service_id | number | ✅ | ID del servicio |
| message | string | ❌ | Mensaje inicial |

**Ejemplo de solicitud:**

```json
{
  "service_id": 1,
  "message": "Necesito que desarrolles mi página web..."
}
```

**Respuesta exitosa (201):**

```json
{
  "success": true,
  "data": {
    "order": {
      "id": 1,
      "service_id": 1,
      "buyer_id": 2,
      "seller_id": 1,
      "status": "pending",
      "total_amount": 150,
      "created_at": "2024-03-15T10:30:00Z"
    }
  }
}
```

---

## 💬 Mensajes

### Enviar Mensaje

**POST** `/api/messages`

Envía un mensaje a otro usuario.

**Headers:**
```
Authorization: Bearer {token}
```

**Parámetros del cuerpo:**

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| receiver_id | number | ✅ | ID del destinatario |
| order_id | number | ❌ | ID de la orden relacionada |
| content | string | ✅ | Contenido del mensaje |

**Ejemplo de solicitud:**

```json
{
  "receiver_id": 1,
  "order_id": 1,
  "content": "Hola, me interesa tu servicio..."
}
```

**Respuesta exitosa (201):**

```json
{
  "success": true,
  "data": {
    "message": {
      "id": 1,
      "sender_id": 2,
      "receiver_id": 1,
      "order_id": 1,
      "content": "Hola, me interesa tu servicio...",
      "read_at": null,
      "created_at": "2024-03-15T10:30:00Z"
    }
  }
}
```

---

## 🔧 Códigos de Error

### Errores de Validación (422)

```json
{
  "success": false,
  "error": {
    "message": "Validation failed",
    "code": "VALIDATION_ERROR",
    "details": [
      {
        "field": "email",
        "message": "El email es requerido"
      },
      {
        "field": "password",
        "message": "La contraseña debe tener al menos 6 caracteres"
      }
    ]
  }
}
```

### Error de Autenticación (401)

```json
{
  "success": false,
  "error": {
    "message": "Token no válido o expirado",
    "code": "UNAUTHORIZED"
  }
}
```

### Error de Permisos (403)

```json
{
  "success": false,
  "error": {
    "message": "No tienes permisos para realizar esta acción",
    "code": "FORBIDDEN"
  }
}
```

### Recurso No Encontrado (404)

```json
{
  "success": false,
  "error": {
    "message": "El recurso solicitado no existe",
    "code": "NOT_FOUND"
  }
}
```
