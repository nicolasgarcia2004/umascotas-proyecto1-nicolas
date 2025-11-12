# 🔒 NOTAS DE SEGURIDAD Y CONFIGURACIÓN

Este documento describe las configuraciones sensibles que NO fueron incluidas en el repositorio por razones de seguridad.

---

## ⚠️ ARCHIVOS EXCLUIDOS DEL REPOSITORIO

Los siguientes archivos contienen información sensible y NO deben ser subidos a GitHub:

### 1. `src/main/resources/application.properties`
**Razón:** Contiene credenciales de base de datos  
**Solución:** Se proporciona `application.properties.example` como plantilla

**Contenido original (sanitizado):**
```properties
spring.datasource.username=${DB_USER:root}
spring.datasource.password=${DB_PASS:****}  # Contraseña removida por seguridad
```

---

## 🔐 INFORMACIÓN SENSIBLE REMOVIDA

### Credenciales de Base de Datos
- **Usuario MySQL:** No incluido (usar variable de entorno `DB_USER`)
- **Contraseña MySQL:** No incluida (usar variable de entorno `DB_PASS`)
- **Host/Puerto:** localhost:3306 (puede configurarse según entorno)

### Secretos JWT
- La clave secreta JWT se genera automáticamente en `JwtUtil.java`
- En producción, esta debe ser una variable de entorno

---

## 📝 INSTRUCCIONES DE CONFIGURACIÓN

### Paso 1: Crear archivo de configuración
```bash
cd src/main/resources
cp application.properties.example application.properties
```

### Paso 2: Configurar credenciales
Editar `application.properties` y reemplazar:
```properties
spring.datasource.username=tu_usuario_mysql
spring.datasource.password=tu_contraseña_mysql
```

### Paso 3: Crear base de datos
```sql
CREATE DATABASE umascotas CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Paso 4: Configurar variables de entorno (RECOMENDADO)
En lugar de editar el archivo, usar variables de entorno:

**Windows (PowerShell):**
```powershell
$env:DB_USER="root"
$env:DB_PASS="tu_contraseña"
```

**Linux/Mac:**
```bash
export DB_USER="root"
export DB_PASS="tu_contraseña"
```

**IntelliJ IDEA / Eclipse:**
- Configurar en Run Configurations → Environment Variables
- Agregar: `DB_USER=root;DB_PASS=tu_contraseña`

---

## 🛡️ BUENAS PRÁCTICAS DE SEGURIDAD

### ✅ LO QUE HICIMOS BIEN

1. **Gitignore Configurado:**
   - Excluye `application.properties`
   - Excluye archivos compilados (`target/`, `*.class`)
   - Excluye configuraciones de IDE

2. **Variables de Entorno:**
   - Uso de `${DB_USER:root}` para valores por defecto
   - Permite sobrescribir con variables de entorno

3. **Contraseñas Hasheadas:**
   - BCrypt con 10 rondas de salt
   - Nunca se almacenan contraseñas en texto plano

4. **JWT con Clave Secreta:**
   - Se genera automáticamente una clave segura
   - Token con expiración de 6 horas

### ⚠️ LO QUE SE DEBE MEJORAR EN PRODUCCIÓN

1. **Migrar a Variables de Entorno Completas:**
   ```properties
   spring.datasource.url=${DATABASE_URL}
   jwt.secret=${JWT_SECRET_KEY}
   jwt.expiration=${JWT_EXPIRATION:21600000}
   ```

2. **Usar Secretos Externos:**
   - AWS Secrets Manager
   - Azure Key Vault
   - HashiCorp Vault

3. **Habilitar HTTPS:**
   ```properties
   server.ssl.enabled=true
   server.ssl.key-store=classpath:keystore.p12
   server.ssl.key-store-password=${SSL_PASSWORD}
   ```

4. **Habilitar CSRF en Producción:**
   ```java
   // En SecurityConfig.java, remover:
   .csrf(csrf -> csrf.disable())
   ```

5. **Configurar CORS Específico:**
   ```java
   @CrossOrigin(origins = {"https://tu-dominio.com"})
   ```

---

## 🗄️ CONFIGURACIÓN DE BASE DE DATOS

### Crear Usuario Específico (Recomendado)
```sql
-- Crear usuario con permisos limitados
CREATE USER 'umascota_user'@'localhost' IDENTIFIED BY 'contraseña_segura_aquí';

-- Otorgar permisos solo a la base de datos específica
GRANT SELECT, INSERT, UPDATE, DELETE ON umascotas.* TO 'umascota_user'@'localhost';

-- Aplicar cambios
FLUSH PRIVILEGES;
```

Luego usar:
```properties
spring.datasource.username=umascota_user
spring.datasource.password=contraseña_segura_aquí
```

---

## 📋 CHECKLIST DE SEGURIDAD ANTES DE DEPLOYMENT

- [ ] Todas las credenciales usan variables de entorno
- [ ] No hay contraseñas hardcodeadas en el código
- [ ] `.gitignore` está correctamente configurado
- [ ] HTTPS está habilitado
- [ ] CORS está configurado para dominios específicos
- [ ] CSRF está habilitado
- [ ] Logs no exponen información sensible
- [ ] Endpoints de actuator están protegidos
- [ ] Base de datos usa usuario con permisos mínimos
- [ ] Contraseñas de BD tienen complejidad adecuada
- [ ] JWT secret es fuerte y único
- [ ] Expiración de tokens configurada
- [ ] Rate limiting implementado
- [ ] Input validation en todos los endpoints

---

## 🚨 EN CASO DE EXPOSICIÓN ACCIDENTAL

Si accidentalmente se sube información sensible a GitHub:

### 1. Cambiar Credenciales Inmediatamente
```sql
ALTER USER 'usuario'@'localhost' IDENTIFIED BY 'nueva_contraseña_segura';
```

### 2. Limpiar Historial de Git
```bash
# Usar BFG Repo-Cleaner o git-filter-branch
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch src/main/resources/application.properties" \
  --prune-empty --tag-name-filter cat -- --all
```

### 3. Forzar Push
```bash
git push origin --force --all
```

### 4. Reportar en GitHub
- Ir a Settings → Security → Secret scanning
- Revisar y revocar secretos expuestos

---

## 📞 CONTACTO EN CASO DE PROBLEMAS DE SEGURIDAD

Si encuentras algún problema de seguridad en este proyecto:
1. NO lo reportes públicamente
2. Contacta al propietario del repositorio directamente
3. Describe el problema con detalles (sin exponerlo públicamente)

---

## 📚 RECURSOS ADICIONALES

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Spring Security Documentation](https://spring.io/projects/spring-security)
- [12 Factor App - Config](https://12factor.net/config)
- [GitHub Secret Scanning](https://docs.github.com/en/code-security/secret-scanning)

---

**Última Actualización:** Noviembre 2025  
**Mantenedor:** Nicolas  
**Propósito:** Documentar medidas de seguridad del proyecto académico
