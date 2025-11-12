# 🤖 PROMPT para Proyecto 2 - U-Mascota Evolution

Este archivo contiene el prompt recomendado para generar el **Proyecto 2** de la asignatura de Entornos de Programación, que consiste en migrar el sistema U-Mascota a una arquitectura moderna con **React + Spring Boot + MongoDB**.

---

## 📝 PROMPT PARA IA GENERADORA

```
Necesito crear el Proyecto 2 de mi asignatura de Entornos de Programación. 
Este proyecto debe ser una evolución del Proyecto 1 (U-Mascota), migrando 
la arquitectura a tecnologías modernas.

CONTEXTO DEL PROYECTO 1:
- Repositorio: https://github.com/nicolasgarcia2004/umascotas-proyecto1-nicolas
- Es un sistema de adopción de mascotas
- Stack actual: Spring Boot + MySQL + Thymeleaf
- Funcionalidades: Registro de usuarios (Adoptantes/Publicadores), 
  CRUD de mascotas, autenticación con JWT, gestión de adopciones

REQUISITOS DEL PROYECTO 2:

TECNOLOGÍAS OBLIGATORIAS:
1. Frontend: React.js (SPA - Single Page Application)
   - Usar React Hooks (useState, useEffect, useContext)
   - React Router para navegación
   - Axios para llamadas HTTP
   - Context API o Redux para estado global
   - Diseño responsivo (Material-UI o Bootstrap React)

2. Backend: Spring Boot 3.x
   - Java 21
   - API RESTful
   - Spring Security + JWT
   - Spring Data MongoDB
   - Validación de datos con Bean Validation

3. Base de Datos: MongoDB
   - Migrar modelo relacional a documentos NoSQL
   - Colecciones: usuarios, mascotas, solicitudes, adopciones
   - Uso de referencias y documentos embebidos según corresponda

FUNCIONALIDADES A IMPLEMENTAR:

Desde el Proyecto 1 (MANTENER):
✅ Registro e inicio de sesión de usuarios
✅ Roles: Adoptante y Publicador
✅ CRUD de mascotas (crear, leer, actualizar, eliminar)
✅ Búsqueda y filtrado de mascotas
✅ Cambio de estado de mascotas (disponible/adoptado/pendiente)

Nuevas Funcionalidades (AGREGAR):
🆕 Sistema de mensajería entre adoptante y publicador
🆕 Dashboard con estadísticas:
   - Total de mascotas publicadas
   - Total de adopciones completadas
   - Gráficos de mascotas por especie
   - Tendencias de adopción
🆕 Sistema de solicitudes de adopción:
   - Formulario detallado para solicitar adopción
   - Seguimiento de estado de solicitud
   - Notificaciones de cambio de estado
🆕 Perfil de usuario mejorado:
   - Edición de datos personales
   - Historial de publicaciones (publicadores)
   - Historial de adopciones (adoptantes)
🆕 Galería de múltiples fotos por mascota
🆕 Sistema de filtros avanzados:
   - Por edad, especie, tamaño, ubicación
   - Búsqueda por texto en descripción
🆕 Validaciones mejoradas en formularios
🆕 Mensajes de error y éxito con toasts/notificaciones

ARQUITECTURA ESPERADA:

Frontend (React):
├── src/
│   ├── components/          # Componentes reutilizables
│   │   ├── common/         # Botones, inputs, cards, etc.
│   │   ├── layout/         # Header, Footer, Sidebar
│   │   └── mascotas/       # Componentes específicos de mascotas
│   ├── pages/              # Páginas principales (vistas)
│   │   ├── Home.jsx
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Dashboard.jsx
│   │   ├── MascotasList.jsx
│   │   └── MascotaDetail.jsx
│   ├── services/           # Servicios de API (axios)
│   │   ├── authService.js
│   │   └── mascotaService.js
│   ├── context/            # Context API para estado global
│   │   └── AuthContext.jsx
│   ├── utils/              # Utilidades y helpers
│   ├── App.jsx
│   └── main.jsx

Backend (Spring Boot):
├── src/main/java/com/umascota/
│   ├── UmascotaApplication.java
│   ├── config/
│   │   ├── SecurityConfig.java
│   │   └── MongoConfig.java
│   ├── controller/
│   │   ├── AuthController.java
│   │   ├── MascotaController.java
│   │   ├── SolicitudController.java
│   │   └── MensajeController.java
│   ├── model/
│   │   ├── Usuario.java
│   │   ├── Mascota.java
│   │   ├── Solicitud.java
│   │   └── Mensaje.java
│   ├── repository/
│   │   ├── UsuarioRepository.java
│   │   ├── MascotaRepository.java
│   │   ├── SolicitudRepository.java
│   │   └── MensajeRepository.java
│   ├── service/
│   │   ├── AuthService.java
│   │   ├── MascotaService.java
│   │   └── SolicitudService.java
│   ├── dto/                # Data Transfer Objects
│   ├── exception/          # Manejo de excepciones
│   └── util/
│       └── JwtUtil.java

MODELO DE DATOS MONGODB:

Colección: usuarios
{
  "_id": ObjectId,
  "nombreCompleto": String,
  "correoElectronico": String (único),
  "contrasena": String (hasheada),
  "telefono": String,
  "ciudad": String,
  "fechaRegistro": Date,
  "tipoUsuario": String ("ADOPTANTE" | "PUBLICADOR"),
  "fotoPerfil": String,
  "activo": Boolean
}

Colección: mascotas
{
  "_id": ObjectId,
  "nombre": String,
  "especie": String,
  "raza": String,
  "edad": Number,
  "descripcion": String,
  "estadoSalud": String,
  "fotos": [String],  // Array de URLs
  "estadoPublicacion": String ("disponible" | "adoptado" | "pendiente"),
  "idUsuarioPublica": ObjectId (ref: usuarios),
  "fechaPublicacion": Date,
  "ubicacion": {
    "ciudad": String,
    "direccion": String
  }
}

Colección: solicitudes
{
  "_id": ObjectId,
  "idMascota": ObjectId (ref: mascotas),
  "idAdoptante": ObjectId (ref: usuarios),
  "idPublicador": ObjectId (ref: usuarios),
  "mensaje": String,
  "estado": String ("pendiente" | "aprobada" | "rechazada"),
  "fechaSolicitud": Date,
  "fechaRespuesta": Date
}

Colección: mensajes
{
  "_id": ObjectId,
  "idEmisor": ObjectId (ref: usuarios),
  "idReceptor": ObjectId (ref: usuarios),
  "idMascota": ObjectId (ref: mascotas),
  "contenido": String,
  "leido": Boolean,
  "fechaEnvio": Date
}

ENDPOINTS API REST:

Autenticación:
POST   /api/auth/registro
POST   /api/auth/login
POST   /api/auth/logout
GET    /api/auth/perfil

Mascotas:
GET    /api/mascotas               # Listar todas
GET    /api/mascotas/{id}          # Obtener por ID
GET    /api/mascotas/disponibles   # Solo disponibles
POST   /api/mascotas               # Crear (solo publicadores)
PUT    /api/mascotas/{id}          # Actualizar
DELETE /api/mascotas/{id}          # Eliminar
GET    /api/mascotas/usuario/{id}  # Mascotas de un usuario

Solicitudes:
GET    /api/solicitudes            # Listar solicitudes
POST   /api/solicitudes            # Crear solicitud
PUT    /api/solicitudes/{id}       # Actualizar estado
GET    /api/solicitudes/mascota/{id}  # Por mascota
GET    /api/solicitudes/usuario/{id}  # Por usuario

Mensajes:
GET    /api/mensajes/conversacion/{idUsuario}/{idMascota}
POST   /api/mensajes
PUT    /api/mensajes/{id}/leer

Dashboard:
GET    /api/dashboard/estadisticas

CONFIGURACIÓN Y SEGURIDAD:

1. CORS: Permitir peticiones desde http://localhost:5173 (Vite) o 3000 (Create React App)
2. JWT: Token con expiración de 24 horas
3. Passwords: BCrypt con salt de 10 rondas
4. Variables de entorno (.env):
   - MONGODB_URI
   - JWT_SECRET
   - PORT
5. Validaciones: Bean Validation en backend, validación de formularios en React

EXTRAS OPCIONALES (Si hay tiempo):
- Subida de imágenes (almacenamiento en cloud: Cloudinary, AWS S3)
- Paginación en listados
- Búsqueda en tiempo real (debounce)
- Modo oscuro/claro
- Notificaciones en tiempo real (WebSockets o Socket.io)
- PWA (Progressive Web App)
- Docker y docker-compose para deployment
- Tests unitarios (JUnit en backend, Jest en frontend)

ENTREGABLES:
1. Código fuente en repositorio GitHub
2. README.md con:
   - Instrucciones de instalación
   - Configuración de variables de entorno
   - Comandos para ejecutar el proyecto
   - Capturas de pantalla
3. Documentación de API (Swagger/OpenAPI)
4. Script SQL/MongoDB para poblar base de datos de prueba
5. Video demo de 5-10 minutos mostrando funcionalidades

RESTRICCIONES:
- NO usar frameworks de backend que no sean Spring Boot
- NO usar bases de datos relacionales (solo MongoDB)
- Código limpio y comentado
- Seguir convenciones de nombres (camelCase en Java/JS)
- Estructura de carpetas organizada

Por favor, genera la estructura completa del proyecto con:
- Configuración inicial de ambos proyectos (frontend y backend)
- Código funcional para al menos 3-4 funcionalidades principales
- Archivos de configuración necesarios
- README.md con instrucciones detalladas
```

---

## 🎯 Instrucciones de Uso del Prompt

1. **Copiar el prompt completo** desde los bloques de código arriba
2. **Pegar en tu IA favorita** (ChatGPT, Claude, Copilot, etc.)
3. **Esperar a que genere** la estructura del proyecto
4. **Revisar y ajustar** según necesidades específicas
5. **Implementar** el código generado

---

## 📌 Notas Importantes

- Este prompt está diseñado para ser lo más específico posible
- Incluye toda la información del Proyecto 1 como contexto
- Define claramente las tecnologías y arquitectura esperada
- Proporciona ejemplos de estructura de datos y endpoints
- Es flexible para agregar o quitar funcionalidades según el tiempo disponible

---

## ✅ Checklist de Verificación

Antes de considerar el Proyecto 2 completo, verificar:

- [ ] Frontend React funcionando en puerto 5173 o 3000
- [ ] Backend Spring Boot funcionando en puerto 8080
- [ ] MongoDB corriendo y conectado
- [ ] Autenticación JWT implementada
- [ ] Al menos 5 funcionalidades principales trabajando
- [ ] CORS configurado correctamente
- [ ] Variables de entorno configuradas
- [ ] README con instrucciones claras
- [ ] Código subido a GitHub
- [ ] Demo funcional del sistema

---

**Creado para facilitar la migración del Proyecto 1 al Proyecto 2**  
**Asignatura:** Entornos de Programación  
**Desarrollador:** Nicolas  
**Fecha:** Noviembre 2025
