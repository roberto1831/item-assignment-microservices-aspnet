# 🧑‍💻 Prueba Técnica – Confiamed Backend (Microservicios)

![.NET](https://img.shields.io/badge/.NET_8-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=csharp&logoColor=white)
![ASP.NET Core](https://img.shields.io/badge/ASP.NET_Core-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Razor Pages](https://img.shields.io/badge/Razor_Pages-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)

> Solución de microservicios en **.NET 8** con cliente web en **ASP.NET Core Razor Pages**. Implementa asignación inteligente de ítems a usuarios con reglas de negocio basadas en carga y prioridad.

---

## 🏗️ Arquitectura del Sistema

```
┌──────────────────┐     GET /api/usuarios     ┌──────────────────┐
│  Usuario_service │◀──────────────────────────│   Item_service   │
│   API REST       │                           │   API REST       │
│  :7057           │                           │  :7058           │
└──────────────────┘                           └────────┬─────────┘
                                                        │
                                               ┌────────▼─────────┐
                                               │  WebCliente      │
                                               │  Razor Pages     │
                                               │  :5216           │
                                               └──────────────────┘
```

| Proyecto | Tecnología | Puerto | Responsabilidad |
|---|---|---|---|
| **Usuario_service** | ASP.NET Core API | 7057 | Expone y rota usuarios disponibles |
| **Item_service** | ASP.NET Core API | 7058 | Gestiona ítems y aplica reglas de asignación |
| **WebCliente** | Razor Pages | 5216 | Interfaz web para usuarios e ítems |

---

## 📜 Reglas de Negocio – Asignación de Ítems

```
¿Fecha de entrega < 3 días?
        │
       SÍ ──────────────────▶ Asignar al usuario con MENOS carga
        │
        NO
        │
¿Relevancia ALTA?
        │
       SÍ ──────────────────▶ Asignar al usuario con menos de 3 ítems de alta prioridad
        │
        NO
        │
        └───────────────────▶ Asignar al usuario con MENOS carga general
```

---

## ⚙️ Requisitos

- .NET 8 SDK
- Visual Studio 2022 (o VS Code con extensiones C# y Razor)
- Postman (para pruebas de API)

---

## ▶️ Ejecución

El proyecto está configurado para levantar los **3 microservicios simultáneamente**:

```bash
# 1. Abrir la solución
WebCliente.sln

# 2. Seleccionar perfil "Nuevo perfil" en Visual Studio
# 3. Ejecutar con Ctrl+F5 o F5
```

### URLs de acceso

| Servicio | URL |
|---|---|
| Usuario_service (Swagger) | https://localhost:7057/swagger/index.html |
| Item_service (Swagger) | https://localhost:7058/swagger/index.html |
| WebCliente | http://localhost:5216 |

---

## 📌 Endpoints

### 🔹 Usuario_service

| Método | Endpoint | Descripción |
|---|---|---|
| GET | `/api/usuarios` | Lista todos los usuarios |
| GET | `/api/usuarios/{nombre}` | Obtiene un usuario específico |
| GET | `/api/usuarios/asignar` | Rota y devuelve el siguiente usuario disponible |

**Ejemplo de respuesta:**
```json
[
  { "id": 1, "nombreUsuario": "Juan Diaz", "totalItemsAsignados": 3, "itemsAltaPrioridad": 1 },
  { "id": 2, "nombreUsuario": "Maria Toques", "totalItemsAsignados": 5, "itemsAltaPrioridad": 2 },
  { "id": 3, "nombreUsuario": "Pedro Estevez", "totalItemsAsignados": 2, "itemsAltaPrioridad": 0 }
]
```

---

### 🔹 Item_service

| Método | Endpoint | Descripción |
|---|---|---|
| POST | `/api/items/asignar` | Asigna un ítem automáticamente según reglas de negocio |
| GET | `/api/items/pendientes` | Devuelve ítems no completados |
| POST | `/api/items/completar/{id}` | Marca un ítem como completado |

**Request – Crear ítem:**
```json
{
  "id": 1,
  "titulo": "Reporte Urgente",
  "usuarioAsignado": "",
  "fechaEntrega": "2025-09-20T00:00:00",
  "completado": false,
  "relevancia": 1
}
```

**Respuesta:**
```json
{
  "usuario": "Juan Diaz",
  "item": "Reporte Urgente"
}
```

---

### 🔹 WebCliente (Razor Pages)

| Vista | Descripción |
|---|---|
| **Usuarios** | Lista de usuarios con carga actual desde Usuario_service |
| **Items** | Ítems pendientes, creación y marcado como completados |

---

## 🧪 Pruebas con Postman

```bash
# Crear ítem
POST https://localhost:7058/api/items/asignar
Content-Type: application/json

{
  "id": 2,
  "titulo": "Tarea crítica",
  "usuarioAsignado": "",
  "fechaEntrega": "2025-09-18T00:00:00",
  "completado": false,
  "relevancia": 1
}

# Consultar pendientes
GET https://localhost:7058/api/items/pendientes

# Completar un ítem
POST https://localhost:7058/api/items/completar/1
```

---

## 👤 Autor

**Ing. Roberto Toapanta**  
📍 Quito, Ecuador  
🔗 [GitHub](https://github.com/roberto1831) · [LinkedIn](https://linkedin.com/in/roberto1831)

---

## 📄 Licencia

Uso académico / demostrativo. No apto para producción sin revisión de seguridad.
