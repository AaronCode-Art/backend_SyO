<div align="center">

# SyO — Backend API

**Plataforma profesional de e-commerce para repuestos y componentes tecnológicos**

![Java](https://img.shields.io/badge/Java-21-orange?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.2.5-green?style=for-the-badge&logo=springboot&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/NeonDB-PostgreSQL_Cloud-00e599?style=for-the-badge&logo=postgresql&logoColor=white)
![Security](https://img.shields.io/badge/Auth-JWT_Stateless-blue?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![GitFlow](https://img.shields.io/badge/Git_Flow-Enabled-cyan?style=for-the-badge&logo=git&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

[Explorar endpoints →](#-endpoints-principales) · [Guía de instalación →](#-guía-de-instalación-local) · [Arquitectura →](#-arquitectura-del-sistema)

</div>

---

## 📖 Descripción General

**SyO** es una plataforma de comercio electrónico especializada en la gestión, venta y distribución de **repuestos y componentes tecnológicos** para PCs, laptops y dispositivos móviles. Este repositorio contiene el núcleo del **Backend**, construido sobre una arquitectura REST robusta, segura y escalable.

El sistema expone una API RESTful que permite gestionar el ciclo de vida completo de un e-commerce: desde el registro de clientes y navegación de catálogo, hasta la gestión de carritos, procesamiento de pedidos y control de estados de entrega.

---

## ✨ Características Principales

- 🔐 **Autenticación stateless** con JWT (registro, login, validación de token)
- 🛒 **Gestión de carrito** en tiempo real por cliente autenticado
- 📦 **Catálogo de productos** con categorías y stock controlado
- 📋 **Ciclo de pedidos** con historial de estados (creación → pago → envío → entrega)
- 📊 **Documentación interactiva** via Swagger UI (Springdoc OpenAPI)
- 🗄️ **Soporte JSONB** en PostgreSQL para datos dinámicos (historial, metadatos)
- ✅ **Validación estricta** de payloads de entrada con Bean Validation

---

## 🧱 Arquitectura del Sistema

El proyecto sigue una **arquitectura en capas desacoplada** (Layered Architecture), separando responsabilidades de forma clara:

```
┌─────────────────────────────────────────────┐
│              Cliente / Frontend              │
└────────────────────┬────────────────────────┘
                     │ HTTP Requests (JSON)
┌────────────────────▼────────────────────────┐
│          Controller Layer (REST)            │  ← Recibe y responde peticiones HTTP
├─────────────────────────────────────────────┤
│           Service Layer (Business)          │  ← Lógica de negocio e interfaces
├─────────────────────────────────────────────┤
│         Repository Layer (Data JPA)         │  ← Acceso a base de datos
├─────────────────────────────────────────────┤
│        Model Layer (JPA Entities)           │  ← Mapeo objeto-relacional (ORM)
└────────────────────┬────────────────────────┘
                     │
┌────────────────────▼────────────────────────┐
│          NeonDB (PostgreSQL Cloud)          │
└─────────────────────────────────────────────┘
```

### Flujo de Seguridad JWT

```
Request → JwtRequestFilter → [Valida Token] → Controller → Service → Repository
                ↑
           JwtUtil.java
     (Firma, emisión y parseo
          de claims JWT)
```

---

## 📂 Estructura del Proyecto

```text
src/main/java/com/ecomerce/syo/
│
├── config/
│   └── SecurityConfig.java          # Configuración de Spring Security, CORS y políticas
│
├── controller/                      # Capa de exposición — Endpoints REST
│   ├── CarritoController.java
│   ├── CategoriaController.java
│   ├── ClienteController.java
│   ├── PedidoController.java
│   └── ProductoController.java
│
├── dto/                             # Data Transfer Objects (entrada/salida de la API)
│   ├── carrito/
│   │   ├── CarritoItemDTO.java
│   │   └── CarritoResponseDTO.java
│   ├── categoria/
│   │   ├── CategoriaDetalleDTO.java
│   │   └── CategoriaListaDTO.java
│   ├── clientes/
│   │   ├── AuthResponseDTO.java
│   │   ├── LoguinClienteDTO.java
│   │   ├── PerfilClienteDTO.java
│   │   ├── RegistroClienteDTO.java
│   │   └── UpdateClienteDTO.java
│   ├── pedido/
│   │   ├── CrearPedidoRequestDTO.java
│   │   ├── DetallePedidoDTO.java
│   │   ├── DetallePedidoResponseDTO.java
│   │   └── PedidoResponseDTO.java
│   └── producto/
│       ├── ProductoDetalleDTO.java
│       └── ProductoListaDTO.java
│
├── exception/
│   └── ResourceNotFoundException.java   # Manejo personalizado de excepciones
│
├── model/                           # Entidades JPA mapeadas a PostgreSQL
│   ├── Carrito.java
│   ├── CarritoDetalle.java
│   ├── Categoria.java
│   ├── Cliente.java
│   ├── DetallePedido.java
│   ├── EstadoPedido.java
│   ├── HistorialEstadoPedido.java
│   ├── Pago.java
│   ├── Pedido.java
│   └── Producto.java
│
├── repository/                      # Interfaces Spring Data JPA
│   ├── CarritoRepository.java
│   ├── CategoriaRepository.java
│   ├── ClienteRepository.java
│   ├── PedidoRepository.java
│   └── ProductoRepository.java
│
├── security/                        # Filtros y utilidades de seguridad JWT
│   ├── JwtRequestFilter.java        # Interceptor HTTP — valida firma del token
│   └── JwtUtil.java                 # Genera, firma y parsea tokens JWT
│
└── services/                        # Capa de Lógica de Negocio
    ├── impl/
    │   ├── CarritoServiceImpl.java
    │   ├── CategoriaServiceImpl.java
    │   ├── ClienteServiceImpl.java
    │   ├── PedidoServiceImpl.java
    │   └── ProductoServiceImpl.java
    ├── CarritoService.java
    ├── CategoriaService.java
    ├── ClienteService.java
    ├── PedidoService.java
    └── ProductoService.java
```

---

## 📦 Stack Tecnológico

| Categoría | Tecnología | Versión | Propósito |
|---|---|---|---|
| Lenguaje | Java | 21 | Base del proyecto |
| Framework | Spring Boot | 3.2.5 | Núcleo de la aplicación |
| API REST | Spring Web | — | Controladores HTTP y routing |
| Validación | Spring Validation | — | Validación de DTOs (`@NotNull`, `@Email`, etc.) |
| ORM | Spring Data JPA + Hibernate | — | Mapeo objeto-relacional |
| Base de datos | PostgreSQL (NeonDB) | — | Persistencia en la nube |
| Driver DB | PostgreSQL Driver | — | Conector nativo JDBC |
| JSONB | Hypersistence Utils | 3.7.3 | Soporte de tipos complejos PostgreSQL |
| Serialización | Jackson Databind | — | Serialización/deserialización JSON |
| Seguridad | Spring Security | — | Seguridad de endpoints y CORS |
| Autenticación | JJWT | 0.12.6 | Firma, emisión y validación de JWT |
| Documentación | Springdoc OpenAPI | 2.5.0 | Swagger UI interactivo |
| Productividad | Lombok | — | Reducción de boilerplate |
| Testing | Spring Boot Test + Security Test | — | Tests unitarios e integración |

---

## 🌐 Endpoints Principales

> La documentación interactiva completa está disponible en `/swagger-ui/index.html` tras levantar el servidor.

### 🔑 Autenticación — `/api/clientes`

| Método | Ruta | Descripción | Auth |
|--------|------|-------------|------|
| `POST` | `/api/clientes/registro` | Registro de nuevo cliente | No |
| `POST` | `/api/clientes/login` | Login — retorna token JWT | No |
| `GET` | `/api/clientes/perfil` | Ver perfil del cliente autenticado | ✅ JWT |
| `PUT` | `/api/clientes/actualizar` | Actualizar datos del perfil | ✅ JWT |

### 🛍️ Productos — `/api/productos`

| Método | Ruta | Descripción | Auth |
|--------|------|-------------|------|
| `GET` | `/api/productos` | Listar todos los productos | No |
| `GET` | `/api/productos/{id}` | Detalle de un producto | No |

### 📂 Categorías — `/api/categorias`

| Método | Ruta | Descripción | Auth |
|--------|------|-------------|------|
| `GET` | `/api/categorias` | Listar categorías | No |
| `GET` | `/api/categorias/{id}` | Detalle de categoría | No |

### 🛒 Carrito — `/api/carrito`

| Método | Ruta | Descripción | Auth |
|--------|------|-------------|------|
| `GET` | `/api/carrito` | Ver carrito del cliente | ✅ JWT |
| `POST` | `/api/carrito/agregar` | Agregar ítem al carrito | ✅ JWT |
| `DELETE` | `/api/carrito/eliminar/{id}` | Eliminar ítem del carrito | ✅ JWT |

### 📋 Pedidos — `/api/pedidos`

| Método | Ruta | Descripción | Auth |
|--------|------|-------------|------|
| `POST` | `/api/pedidos` | Crear pedido desde el carrito | ✅ JWT |
| `GET` | `/api/pedidos` | Listar pedidos del cliente | ✅ JWT |
| `GET` | `/api/pedidos/{id}` | Detalle de un pedido | ✅ JWT |

---

## 🚀 Guía de Instalación Local

### Prerrequisitos

Asegúrate de tener instalado lo siguiente:

- [Java 21](https://adoptium.net/) (JDK)
- [Maven 3.8+](https://maven.apache.org/download.cgi) o usar el wrapper incluido (`./mvnw`)
- Una base de datos PostgreSQL activa (local o en la nube — se recomienda [NeonDB](https://neon.tech))
- [Git](https://git-scm.com/)

### 1. Clonar el repositorio

```bash
git clone https://github.com/AaronCode-Art/backend_SyO.git
cd backend_SyO
```

### 2. Configurar variables de entorno

Crea el archivo `src/main/resources/application.properties` (o `application.yml`) con tu configuración:

```properties
# Base de datos PostgreSQL (NeonDB)
spring.datasource.url=jdbc:postgresql://<host>/<database>?sslmode=require
spring.datasource.username=<tu_usuario>
spring.datasource.password=<tu_contraseña>
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# JWT
jwt.secret=<tu_clave_secreta_de_al_menos_256_bits>
jwt.expiration=86400000

# Puerto del servidor
server.port=8080
```

> ⚠️ **Nunca subas `application.properties` con credenciales reales al repositorio.** Agrégalo al `.gitignore` o usa variables de entorno del sistema.

### 3. Construir el proyecto

```bash
# Con el wrapper de Maven (no requiere Maven instalado)
./mvnw clean install

# O con Maven instalado globalmente
mvn clean install
```

### 4. Ejecutar el servidor

```bash
./mvnw spring-boot:run
```

El servidor quedará disponible en: `http://localhost:8080`

### 5. Explorar la documentación Swagger

Una vez corriendo, abre en tu navegador:

```
http://localhost:8080/swagger-ui/index.html
```

Desde ahí puedes probar todos los endpoints directamente, incluyendo los autenticados (necesitas primero hacer login y copiar el token JWT).

---

## 🔐 Uso de la Autenticación JWT

1. **Registra un usuario** en `POST /api/clientes/registro`
2. **Haz login** en `POST /api/clientes/login` — recibirás un token JWT en la respuesta
3. **Incluye el token** en el header de cada petición protegida:

```http
Authorization: Bearer <tu_token_jwt>
```

En Swagger UI, usa el botón **"Authorize"** (🔒) e ingresa `Bearer <token>` para autenticar todas las peticiones de la sesión.

---

## 🗄️ Modelo de Datos

El esquema principal incluye las siguientes entidades y sus relaciones:

```
Cliente ──────── Carrito ──────── CarritoDetalle ──── Producto
   │                                                      │
   └──── Pedido ──── DetallePedido ───────────────────────┘
             │
             ├──── Pago
             ├──── EstadoPedido
             └──── HistorialEstadoPedido
                         │
                    Categoria
```

---

## 🌿 Flujo de Trabajo Git (GitFlow)

El proyecto usa **GitFlow** como estrategia de branching:

| Rama | Propósito |
|------|-----------|
| `main` | Código estable listo para producción |
| `develop` | Integración de nuevas funcionalidades |
| `feature/*` | Desarrollo de nuevas características |
| `hotfix/*` | Correcciones urgentes en producción |
| `release/*` | Preparación de versiones |

```bash
# Crear una nueva feature
git checkout develop
git checkout -b feature/nombre-feature

# Al finalizar, merge a develop
git checkout develop
git merge feature/nombre-feature
```

---

## 🧪 Ejecutar Tests

```bash
# Ejecutar todos los tests
./mvnw test

# Ejecutar tests con reporte de cobertura
./mvnw test jacoco:report
```

---

## 📁 Variables de Entorno — Referencia

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `SPRING_DATASOURCE_URL` | URL de conexión JDBC | `jdbc:postgresql://...` |
| `SPRING_DATASOURCE_USERNAME` | Usuario de la DB | `neonuser` |
| `SPRING_DATASOURCE_PASSWORD` | Contraseña de la DB | `********` |
| `JWT_SECRET` | Clave secreta para firmar tokens | `my-ultra-secret-key-256bits` |
| `JWT_EXPIRATION` | Expiración del token en ms | `86400000` (24h) |
| `SERVER_PORT` | Puerto del servidor | `8080` |

---

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor sigue estos pasos:

1. Haz un **fork** del repositorio
2. Crea tu rama: `git checkout -b feature/mi-mejora`
3. Commitea tus cambios: `git commit -m 'feat: agrega mi mejora'`
4. Sube la rama: `git push origin feature/mi-mejora`
5. Abre un **Pull Request** hacia `develop`

Consulta los [issues abiertos](https://github.com/AaronCode-Art/backend_SyO/issues) para ver qué se está trabajando.

---

## 👤 Autor

**AaronCode-Art**
GitHub: [@AaronCode-Art](https://github.com/AaronCode-Art)

---

<div align="center">

Hecho con ☕ y Spring Boot

</div>
