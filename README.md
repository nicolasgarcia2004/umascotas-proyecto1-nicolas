# 🐾 U-Mascota - Sistema de Adopción de Mascotas (Proyecto 1)

**Proyecto:** Sistema de Gestión de Adopción de Mascotas  
**Asignatura:** Entornos de Programación  
**Desarrollador:** Nicolas y Bryan  
**Estado:** ✅ Funcional y Operativo  

---

## 📋 Descripción del Proyecto

**U-Mascota** es una aplicación web para la gestión de adopción de mascotas que conecta a publicadores que desean dar en adopción sus mascotas con adoptantes interesados. El sistema permite registrar usuarios, publicar mascotas disponibles, buscarlas y gestionar el proceso de adopción.

---

## 🎯 Objetivos del Proyecto

1. Facilitar la adopción responsable de mascotas
2. Conectar publicadores con potenciales adoptantes
3. Gestionar un catálogo de mascotas disponibles
4. Proveer un sistema seguro de autenticación y autorización
5. Ofrecer una interfaz intuitiva y fácil de usar

---

## 🏗️ Arquitectura y Tecnologías

### **Backend**
- **Framework:** Spring Boot 3.5.6
- **Lenguaje:** Java 21
- **Base de Datos:** MySQL 8.x
- **ORM:** Spring Data JPA / Hibernate
- **Seguridad:** Spring Security + JWT (JSON Web Tokens)
- **Gestión de Dependencias:** Maven

### **Frontend**
- **Motor de Plantillas:** Thymeleaf
- **Estilos:** HTML5 + CSS3
- **Cliente Web:** JavaScript (integración con API REST)

### **Dependencias Principales**
```xml
- spring-boot-starter-web
- spring-boot-starter-data-jpa
- spring-boot-starter-security
- spring-boot-starter-thymeleaf
- spring-boot-starter-actuator
- mysql-connector-j
- jjwt-api (0.11.2) - Autenticación JWT
- lombok
```

---

## 📊 Modelo de Datos

### **Entidad: Usuario**
```java
- idUsuario: Long (PK, Auto-increment)
- nombreCompleto: String
- correoElectronico: String (Único)
- contrasena: String (Encriptada)
- telefono: String
- ciudad: String
- fechaRegistro: LocalDateTime (Auto-generada)
- tipoUsuario: Enum (ADOPTANTE, PUBLICADOR)
```

### **Entidad: Mascota**
```java
- idMascota: Long (PK, Auto-increment)
- nombre: String
- especie: String (Perro, Gato, Ave, etc.)
- raza: String
- edad: Integer
- descripcion: String
- estadoSalud: String
- foto: String (URL o ruta)
- estadoPublicacion: Enum (disponible, adoptado, pendiente)
- idUsuarioPublica: Long (FK a Usuario)
```

### **Relaciones**
- Un **Usuario** (Publicador) puede publicar **muchas Mascotas** (1:N)
- Una **Mascota** solo puede ser publicada por **un Usuario** (N:1)

---

## 🔐 Sistema de Autenticación y Seguridad

### **Características de Seguridad**
1. **Encriptación de Contraseñas:** Las contraseñas se almacenan hasheadas (no en texto plano)
2. **JWT (JSON Web Tokens):** Para autenticación stateless
3. **Spring Security:** Configuración de seguridad a nivel de aplicación
4. **Roles de Usuario:**
   - **ADOPTANTE:** Puede buscar y solicitar adopción de mascotas
   - **PUBLICADOR:** Puede publicar mascotas para adopción

### **Endpoints de Autenticación**
```
POST /auth/registro  - Registrar nuevo usuario
POST /auth/login     - Iniciar sesión
```

---

## 🚀 Funcionalidades Principales

### **Para Todos los Usuarios**
- ✅ Registro de cuenta con tipo de usuario (Adoptante/Publicador)
- ✅ Inicio de sesión seguro
- ✅ Visualización de página de inicio/bienvenida

### **Para Adoptantes**
- ✅ Ver catálogo de mascotas disponibles
- ✅ Buscar mascotas por nombre
- ✅ Filtrar mascotas por especie
- ✅ Ver detalles de cada mascota
- ✅ Solicitar adopción de mascotas

### **Para Publicadores**
- ✅ Publicar nuevas mascotas para adopción
- ✅ Editar información de mascotas publicadas
- ✅ Eliminar publicaciones de mascotas
- ✅ Cambiar estado de publicación (disponible/adoptado/pendiente)
- ✅ Ver sus mascotas publicadas

---

## 🌐 API REST Endpoints

### **Autenticación**
| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/auth/registro` | Registrar nuevo usuario |
| POST | `/auth/login` | Iniciar sesión |

### **Gestión de Mascotas**
| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/mascotas` | Listar todas las mascotas |
| GET | `/api/mascotas/{nombre}` | Buscar mascota por nombre |
| GET | `/api/mascotas/disponibles` | Listar mascotas disponibles |
| POST | `/api/mascotas` | Crear nueva mascota |
| PUT | `/api/mascotas/{id}` | Actualizar mascota |
| PUT | `/api/mascotas/{id}/adoptar` | Marcar mascota como adoptada |
| DELETE | `/api/mascotas/{id}` | Eliminar mascota |

### **Vistas Web (Thymeleaf)**
| URL | Vista | Descripción |
|-----|-------|-------------|
| `/` | home.html | Página de inicio |
| `/login` | login.html | Página de login |
| `/registro` | register.html | Página de registro |
| `/adoptante` | adoptante.html | Panel del adoptante |
| `/publicador` | publicador.html | Panel del publicador |
| `/crear-mascota` | crear-mascota.html | Formulario crear mascota |
| `/listar-mascotas` | listar-mascotas.html | Listado de mascotas |

---

## 🗄️ Configuración de Base de Datos

### **Esquema de Base de Datos**
```sql
Database: umascotas

Tablas:
- usuarios
- mascotas
- solicitudes_adopcion (futura implementación)
- adopciones (futura implementación)
```

### **application.properties**
```properties
# Configuración de Base de Datos
spring.datasource.url=jdbc:mysql://localhost:3306/umascotas
spring.datasource.username=${DB_USER:root}
spring.datasource.password=${DB_PASS:your_password}
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA/Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=false

# Thymeleaf
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
spring.thymeleaf.enabled=true
```

**⚠️ IMPORTANTE:** 
- Usar variables de entorno para credenciales de base de datos
- NO subir contraseñas reales al repositorio
- Configurar `DB_USER` y `DB_PASS` como variables de entorno

---

## 📦 Estructura del Proyecto

```
umascota/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/umascota/
│   │   │       ├── Umascota2Application.java (Clase principal)
│   │   │       ├── config/
│   │   │       │   └── SecurityConfig.java (Configuración de seguridad)
│   │   │       ├── controller/
│   │   │       │   ├── AuthController.java (Autenticación)
│   │   │       │   ├── MascotaController.java (API de mascotas)
│   │   │       │   └── ViewController.java (Rutas de vistas)
│   │   │       ├── model/
│   │   │       │   ├── Mascota.java (Entidad Mascota)
│   │   │       │   └── Usuario.java (Entidad Usuario)
│   │   │       ├── repository/
│   │   │       │   ├── MascotaRepository.java
│   │   │       │   └── UsuarioRepository.java
│   │   │       ├── service/
│   │   │       │   └── UsuarioService.java
│   │   │       └── util/
│   │   │           ├── JwtUtil.java (Manejo de JWT)
│   │   │           └── PasswordUtil.java (Encriptación)
│   │   └── resources/
│   │       ├── application.properties
│   │       └── templates/
│   │           └── view/
│   │               ├── home.html
│   │               ├── login.html
│   │               ├── register.html
│   │               ├── adoptante.html
│   │               ├── publicador.html
│   │               ├── crear-mascota.html
│   │               └── listar-mascotas.html
│   └── test/
│       └── java/
│           └── com/example/umascota/
│               └── Umascota2ApplicationTests.java
├── pom.xml
└── README.md
```

---

## ⚙️ Requisitos del Sistema

### **Software Necesario**
- **JDK:** Java 21 o superior
- **Maven:** 3.6+ (incluido con proyecto)
- **MySQL:** 8.0+
- **IDE Recomendado:** Spring Tool Suite, IntelliJ IDEA, o VS Code

### **Configuración Inicial**

1. **Instalar MySQL y crear la base de datos:**
```sql
CREATE DATABASE umascotas;
```

2. **Configurar variables de entorno (opcional):**
```bash
export DB_USER=root
export DB_PASS=tu_contraseña
```

3. **Copiar y configurar application.properties:**
```bash
cp src/main/resources/application.properties.example src/main/resources/application.properties
# Editar application.properties con tus credenciales
```

4. **Compilar el proyecto:**
```bash
mvn clean install
```

5. **Ejecutar la aplicación:**
```bash
java -jar target/umascota-0.0.1-SNAPSHOT.jar
```

O desde el IDE:
```
Run → Spring Boot App
```

6. **Acceder a la aplicación:**
```
http://localhost:8080
```

---

## 🔄 Flujo de Uso del Sistema

### **Flujo del Publicador**
1. Registrarse como **PUBLICADOR**
2. Iniciar sesión
3. Acceder al panel de publicador
4. Publicar mascota con sus datos (nombre, especie, raza, edad, descripción, foto)
5. Ver sus mascotas publicadas
6. Editar o eliminar publicaciones
7. Cambiar estado de mascota cuando sea adoptada

### **Flujo del Adoptante**
1. Registrarse como **ADOPTANTE**
2. Iniciar sesión
3. Acceder al panel de adoptante
4. Ver catálogo de mascotas disponibles
5. Buscar/filtrar mascotas
6. Ver detalles de mascota de interés
7. Solicitar adopción

---

## 🧪 Testing y Validación

### **Endpoints de Monitoreo**
```
GET /actuator/health - Estado de la aplicación
```

### **Pruebas Realizadas**
- ✅ Registro de usuarios (Adoptante y Publicador)
- ✅ Autenticación y login
- ✅ CRUD de mascotas
- ✅ Filtrado de mascotas disponibles
- ✅ Cambio de estado de mascotas
- ✅ Búsqueda por nombre

---

## 🛡️ Consideraciones de Seguridad

1. **Contraseñas Encriptadas:** Uso de BCrypt para almacenar contraseñas
2. **Variables de Entorno:** Credenciales sensibles no hardcodeadas
3. **CSRF Deshabilitado:** Para desarrollo (habilitar en producción)
4. **Spring Security:** Control de acceso a endpoints
5. **Validación de Datos:** Validación en backend y frontend
6. **application.properties NO incluido:** Por seguridad, se debe crear desde el archivo .example

---

## 🚧 Futuras Mejoras (Proyecto 2)

Para el **Proyecto 2** se propone una evolución con las siguientes tecnologías:

### **Stack Tecnológico Proyecto 2**
- **Frontend:** React.js (SPA - Single Page Application)
- **Backend:** Spring Boot (mantenido)
- **Base de Datos:** MongoDB (NoSQL)
- **Autenticación:** JWT + OAuth2
- **Despliegue:** Docker + Kubernetes

### **Nuevas Funcionalidades Propuestas**
- Sistema de mensajería entre adoptante y publicador
- Sistema de notificaciones en tiempo real
- Dashboard con estadísticas
- Sistema de calificaciones y reviews
- Integración con redes sociales
- Geolocalización de mascotas
- Filtros avanzados de búsqueda
- Galería de fotos múltiples por mascota

---

## 📚 Recursos de Aprendizaje

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA](https://spring.io/projects/spring-data-jpa)
- [Thymeleaf Docs](https://www.thymeleaf.org/documentation.html)
- [JWT.io](https://jwt.io/)
- [MySQL Documentation](https://dev.mysql.com/doc/)

---

## 👥 Equipo de Desarrollo

- **Nicolas** - Desarrollador Principal
- **Bryan** - Desarrollador

---

## 📝 Notas Importantes

- Este proyecto fue desarrollado con fines académicos
- La configuración de seguridad está simplificada para desarrollo
- En producción se debe mejorar la seguridad y configurar HTTPS
- Se recomienda usar Docker para facilitar el despliegue
- **IMPORTANTE:** El archivo `application.properties` NO está incluido por seguridad. Debe crearse a partir de `application.properties.example`

---

## 🎓 Contexto Académico

**Asignatura:** Entornos de Programación  
**Proyecto:** 1 de 2  
**Objetivo:** Desarrollar un sistema web completo con Java, Spring Boot y MySQL  
**Próximo Proyecto:** Migración a React + Spring Boot + MongoDB

---

## 🔗 Repositorio del Proyecto

Este repositorio contiene el código fuente completo del Proyecto 1, documentado para facilitar:
- Análisis por parte de herramientas de IA
- Generación de prompts para el Proyecto 2
- Referencia de arquitectura y patrones utilizados
- Migración a nuevas tecnologías (React + MongoDB)

---

## 📞 Soporte y Contacto

Para dudas o consultas sobre este proyecto, contactar a través del repositorio de GitHub.

---

**Última Actualización:** Noviembre 2025  
**Versión:** 1.0.0  
**Licencia:** Proyecto Académico

---

## 🎉 ¡Gracias por visitar U-Mascota!

_"Ayudando a encontrar un hogar para cada mascota 🐶🐱"_
