# 🏎️ Karting Emoción

**Karting Emoción** es una aplicación web fullstack desarrollada en PHP que permite gestionar de forma integral un circuito de karting. La plataforma ofrece una experiencia diferenciada para tres tipos de usuarios: visitantes, socios registrados y administradores.

---

## 📋 Tabla de Contenidos

- [Características](#-características)
- [Arquitectura](#-arquitectura)
- [Tecnologías](#-tecnologías)
- [Requisitos Previos](#-requisitos-previos)
- [Instalación](#-instalación)
- [Base de Datos](#-base-de-datos)
- [Acceso a la Aplicación](#-acceso-a-la-aplicación)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Datos de Prueba](#-datos-de-prueba-opcional)

---

## ✨ Características

### Visitantes (usuarios no registrados)
- Landing page con información del circuito, servicios y pistas disponibles.
- Visualización de precios y packs.
- Carruseles de imágenes de las pistas.
- Sección de contacto y redes sociales.

### Socios registrados
- Registro e inicio de sesión con contraseñas encriptadas (bcrypt).
- Panel personal con acceso a:
  - **Pistas disponibles** — consulta de información detallada de cada circuito.
  - **Precios** — tarifas por pista y duración (10, 20 o 30 minutos).
  - **Ranking de tiempos** — tabla competitiva con tiempos ordenados, con resaltado del tiempo propio.
  - **Mis Reservas** — listado de reservas con estado (pendiente, confirmada, cancelada) y acciones para confirmar o cancelar.
  - **Nueva Reserva** — formulario para reservar pista seleccionando fecha, hora, duración y número de participantes, con cálculo automático del precio total.

### Administrador
- Panel de administración protegido con sidebar de navegación.
- CRUD completo de:
  - **Pistas** — crear, editar, eliminar y listar circuitos.
  - **Precios** — gestionar tarifas por pista y duración.
  - **Reservas** — visualizar y gestionar todas las reservas del sistema.
  - **Tiempos** — registrar, editar y eliminar tiempos de los socios.
  - **Socios** — gestionar usuarios registrados (actualizar, eliminar, buscar).

---

## 🏗️ Arquitectura

El proyecto sigue el patrón **MVC (Modelo-Vista-Controlador)**:

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────┐
│   Vistas    │◄───►│  Controladores   │◄───►│   Modelos   │
│   (PHP/HTML)│     │   (Lógica)       │     │  (MySQL)    │
└─────────────┘     └──────────────────┘     └─────────────┘
```

- **Modelos** — Clases PHP que encapsulan la lógica de acceso a datos con consultas preparadas (MySQLi).
- **Vistas** — Archivos PHP con HTML + Bootstrap para la interfaz de usuario.
- **Controladores** — Gestionan las acciones del usuario, validan sesiones y coordinan modelos y vistas.

---

## 🛠️ Tecnologías

| Tecnología | Uso |
|---|---|
| **PHP 7+** | Backend y lógica de servidor |
| **MySQL** | Base de datos relacional |
| **Bootstrap 5.3** | Framework CSS para diseño responsive |
| **Font Awesome 6** | Iconografía |
| **HTML5 / CSS3** | Estructura y estilos personalizados |
| **XAMPP** | Entorno de desarrollo local (Apache + MySQL) |

---

## 📦 Requisitos Previos

- [XAMPP](https://www.apachefriends.org/) (o cualquier stack con Apache + PHP + MySQL)
- PHP 7.0 o superior
- MySQL 5.7 o superior
- Navegador web moderno

---

## 🚀 Instalación

1. **Clonar el repositorio** dentro de la carpeta `htdocs` de XAMPP:

   ```bash
   cd C:\xampp\htdocs
   git clone https://github.com/Alexdev486/KARTING.git karting
   ```

2. **Iniciar los servicios** de Apache y MySQL desde el panel de control de XAMPP.

3. **Crear la base de datos**:
   - Acceder a phpMyAdmin: [http://localhost/phpmyadmin](http://localhost/phpmyadmin)
   - Crear una nueva base de datos llamada `karting`.
   - Importar el archivo `karting.sql` ubicado en la raíz del proyecto, o ejecutar su contenido en la pestaña SQL.

4. **Acceder a la aplicación**:

   ```
   http://localhost/karting/vistas/index.php
   ```

---

## 🗄️ Base de Datos

La base de datos `karting` contiene 5 tablas relacionadas:

```
socios ──┬──► reservas ◄── pistas
         │                    │
         └──► tiempos  ◄─────┘
                              │
              precios  ◄──────┘
```

| Tabla | Descripción |
|---|---|
| `socios` | Usuarios registrados (incluye el admin con id=0) |
| `pistas` | Circuitos disponibles con nombre y descripción |
| `reservas` | Reservas de socios con pista, fecha, duración, participantes, precio y estado |
| `tiempos` | Registros de tiempos por vuelta de cada socio en cada pista |
| `precios` | Tarifas por pista y duración (10, 20 o 30 minutos) |

### Diagrama Entidad-Relación

| Relación | Tipo |
|---|---|
| `socios` → `reservas` | 1:N (un socio puede tener muchas reservas) |
| `pistas` → `reservas` | 1:N (una pista puede tener muchas reservas) |
| `socios` → `tiempos` | 1:N (un socio puede tener muchos tiempos) |
| `pistas` → `tiempos` | 1:N (una pista puede tener muchos tiempos) |
| `pistas` → `precios` | 1:N (una pista puede tener varios precios según duración) |

---

## 🔑 Acceso a la Aplicación

### Landing page (visitantes)

```
http://localhost/karting/vistas/index.php
```

### Panel de Administrador

Desde la página de login, introducir:

| Campo | Valor |
|---|---|
| Usuario | `admin` |
| Contraseña | `admin` |

### Panel de Socio

1. Acceder al login desde la landing page.
2. Registrar un nuevo usuario en el apartado **"Registrarse"**.
3. Volver al formulario **"Ingresar"** e iniciar sesión con las credenciales registradas.

---

## 📁 Estructura del Proyecto

```
karting/
│
├── bd/
│   └── bd.php                      # Clase de conexión a MySQL
│
├── controladores/
│   ├── admin_header_footer.php     # Header y footer del panel de administración
│   ├── loginController.php         # Controlador de sesiones
│   ├── PistaController.php         # CRUD de pistas (admin)
│   ├── PreciosController.php       # CRUD de precios (admin)
│   ├── ReservasController.php      # CRUD de reservas (admin)
│   ├── SocioController.php         # Gestión de socios, login y registro
│   ├── TiemposController.php       # CRUD de tiempos (admin)
│   └── UsuarioController.php       # Panel del socio logueado
│
├── Css/
│   ├── index.css                   # Estilos de la landing page
│   ├── login.css                   # Estilos del formulario de login
│   └── usuario.css                 # Estilos del panel de socio
│
├── img/                            # Imágenes de las pistas y recursos gráficos
│
├── modelos/
│   ├── modelo_pistas.php           # Modelo de acceso a datos de pistas
│   ├── modelo_precios.php          # Modelo de acceso a datos de precios
│   ├── modelo_reservas.php         # Modelo de acceso a datos de reservas
│   ├── modelo_socios.php           # Modelo de acceso a datos de socios
│   ├── modelo_tiempos.php          # Modelo de acceso a datos de tiempos
│   └── modelo_usuario.php          # Modelo de datos del panel de socio
│
├── vistas/
│   ├── index.php                   # Landing page principal
│   ├── logout.php                  # Cierre de sesión
│   ├── admin/
│   │   └── inicio.php              # Página de inicio del admin
│   ├── pista/
│   │   ├── actualizar_pista.php    # Formulario de edición de pista
│   │   ├── insertar_pista.php      # Formulario de nueva pista
│   │   └── vista_pistas.php        # Listado de pistas
│   ├── precios/
│   │   ├── actualizar_precio.php   # Formulario de edición de precio
│   │   ├── crear_precio.php        # Formulario de nuevo precio
│   │   └── lista_precios.php       # Listado de precios
│   ├── reservas/
│   │   ├── actualizar_reserva.php  # Formulario de edición de reserva
│   │   ├── crear_reserva.php       # Formulario de nueva reserva
│   │   └── lista_reservas.php      # Listado de reservas
│   ├── socio/
│   │   ├── actualizar.php          # Formulario de edición de socio
│   │   ├── login.php               # Formulario de login y registro
│   │   └── vista_socios.php        # Listado de socios (admin)
│   ├── tiempos/
│   │   ├── actualizar_tiempo.php   # Formulario de edición de tiempo
│   │   ├── crear_tiempo.php        # Formulario de nuevo tiempo
│   │   └── lista_tiempos.php       # Listado de tiempos
│   └── usuario/
│       └── usuario_vista.php       # Panel principal del socio logueado
│
├── karting.sql                     # Script SQL de creación de tablas y datos iniciales
└── README.md
```

---

## 🧪 Datos de Prueba (Opcional)

Para probar la aplicación con datos precargados, ejecutar las siguientes sentencias SQL en phpMyAdmin después de importar el esquema:

```sql
-- Insertar socios de prueba
INSERT INTO socios (id, nombre_usuario, contrasena, email, fecha_registro) VALUES
(1, 'usuario1', '$2y$10$example_hash_1', 'usuario1@example.com', NOW()),
(2, 'usuario2', '$2y$10$example_hash_2', 'usuario2@example.com', NOW()),
(3, 'usuario3', '$2y$10$example_hash_3', 'usuario3@example.com', NOW()),
(4, 'usuario4', '$2y$10$example_hash_4', 'usuario4@example.com', NOW()),
(5, 'usuario5', '$2y$10$example_hash_5', 'usuario5@example.com', NOW());

-- Insertar pistas
INSERT INTO pistas (nombre, descripcion) VALUES
('Karting Copo', 'Kartódromo fundado en 2004 con 20.000 m² de superficie. Pista de casi 1 km de recorrido, ancho de 10-12 m y recta de más de 100 m. Incluye circuito de educación vial infantil de 2.500 m².'),
('Karting Roquetas', 'Kartódromo fundado en 1991 con 18.000 m² de superficie. Pista de 860 m de recorrido, ancho de 8-10 m. Recta más larga de aproximadamente 100 m.');

-- Insertar precios
INSERT INTO precios (pista_id, duracion_minutos, precio) VALUES
(1, 10, 15.00),
(1, 20, 25.00),
(1, 30, 35.00),
(2, 10, 12.00),
(2, 20, 22.00),
(2, 30, 35.00);

-- Insertar reservas de ejemplo
INSERT INTO reservas (socio_id, pista_id, fecha_hora, duracion_minutos, numero_participantes, precio_total, estado) VALUES
(1, 1, '2025-03-10 10:00:00', 30, 2, 70.00, 'confirmada'),
(2, 2, '2025-03-11 11:00:00', 20, 4, 88.00, 'pendiente'),
(3, 1, '2025-03-12 12:00:00', 10, 1, 15.00, 'cancelada'),
(4, 2, '2025-03-13 13:00:00', 30, 5, 175.00, 'confirmada'),
(5, 1, '2025-03-14 14:00:00', 20, 3, 75.00, 'pendiente');

-- Insertar tiempos de ejemplo
INSERT INTO tiempos (socio_id, pista_id, tiempo, fecha) VALUES
(1, 1, '00:02:30', '2025-03-10 10:00:00'),
(2, 2, '00:03:15', '2025-03-11 11:00:00'),
(3, 1, '00:04:10', '2025-03-12 12:00:00'),
(4, 2, '00:01:45', '2025-03-13 13:00:00'),
(5, 1, '00:03:50', '2025-03-14 14:00:00');
```

> **Nota:** Los socios de prueba tienen contraseñas hasheadas de ejemplo. Para iniciar sesión con ellos, se recomienda registrarlos desde la interfaz de la aplicación para que la contraseña se almacene correctamente con bcrypt.

---

## 📄 Licencia

Este proyecto ha sido desarrollado con fines educativos.

---

<p align="center">
  Desarrollado por <a href="https://github.com/Alexdev486">Alexdev486</a>
</p>
