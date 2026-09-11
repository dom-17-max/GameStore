# 🎮 GameStore - Sistema de Gestión de Venta de Videojuegos

## 📌 Contexto

**GameStore** es una aplicación de escritorio desarrollada en Java con conexión a una base de datos MySQL, orientada a resolver la gestión de una tienda dedicada a la venta de videojuegos físicos y digitales (títulos como *Minecraft*, *Resident Evil 4*, *Grand Theft Auto V*, *Elden Ring*, *The Legend Of Zelda: Tears of the Kindom*, entre otros).

Actualmente, este tipo de negocios suele llevar el control de su catálogo, clientes y ventas de forma manual o en hojas de cálculo, lo que genera errores de registro, pérdida de información y dificultad para conocer el stock disponible o el historial de compras de un cliente. Esta aplicación busca centralizar esa información en una base de datos relacional, permitiendo un control confiable y eficiente del negocio.

## 📋 Análisis de Requerimientos

### Requerimientos funcionales
- Registrar, editar, eliminar y consultar videojuegos del catálogo (nombre, plataforma, género, precio, stock).
- Registrar, editar, eliminar y consultar clientes.
- Registrar ventas asociando un cliente con uno o varios videojuegos.
- Consultar el detalle de una venta (juegos vendidos, cantidades, subtotales).
- Buscar videojuegos por nombre, categoría o plataforma.
- Controlar el stock disponible de cada videojuego tras cada venta.

### Requerimientos no funcionales
- Interfaz gráfica intuitiva (JFrame) para usuarios sin conocimientos técnicos.
- Validación de datos de entrada (campos vacíos, formatos numéricos, precios y stock no negativos).
- Conexión estable a una base de datos MySQL.
- Persistencia de la información entre sesiones de uso.

## 🗂️ Modelo Lógico

El sistema se estructura en cinco entidades principales relacionadas entre sí:

```
categorias (1) ────< (N) videojuegos
clientes   (1) ────< (N) ventas
ventas     (1) ────< (N) detalle_venta >──── (N) videojuegos
```

- Un **cliente** puede realizar muchas **ventas**.
- Una **venta** puede incluir muchos **videojuegos** (a través de `detalle_venta`), y un mismo videojuego puede aparecer en muchas ventas.
- Un **videojuego** pertenece a una **categoría** (acción, aventura, deportes, plataformas, etc.), pero una categoría agrupa muchos videojuegos.

## 🧾 Descripción de Tablas

| Tabla | Descripción | Campos principales |
|---|---|---|
| `categorias` | Clasifica los videojuegos por género o tipo | `id_categoria`, `nombre_categoria` |
| `videojuegos` | Catálogo de productos disponibles en la tienda | `id_videojuego`, `nombre`, `plataforma`, `precio`, `stock`, `id_categoria` (FK) |
| `clientes` | Registro de clientes de la tienda | `id_cliente`, `nombre`, `apellido`, `cedula`, `telefono`, `email` |
| `ventas` | Cabecera de cada transacción de venta | `id_venta`, `fecha`, `id_cliente` (FK), `total` |
| `detalle_venta` | Detalle de los videojuegos incluidos en cada venta | `id_detalle`, `id_venta` (FK), `id_videojuego` (FK), `cantidad`, `subtotal` |

## 🗄️ Script SQL
CREATE DATABASE IF NOT EXISTS GameStoreBD;

USE GameStoreBD;

CREATE TABLE usuarios (
    id_usuario INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    usuario VARCHAR(50) NOT NULL UNIQUE,
    contrasena VARCHAR(255) NOT NULL
);

CREATE TABLE videojuegos (
    id_videojuego INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    genero VARCHAR(50) NOT NULL,
    plataforma VARCHAR(50) NOT NULL,
    precio DECIMAL(10,2) NOT NULL,
    stock INT NOT NULL DEFAULT 0
);

CREATE TABLE ventas (
    id_venta INT AUTO_INCREMENT PRIMARY KEY,
    fecha DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    id_usuario INT NOT NULL,

    CONSTRAINT fk_venta_usuario
        FOREIGN KEY (id_usuario)
        REFERENCES usuarios(id_usuario)
);

CREATE TABLE detalle_venta (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_venta INT NOT NULL,
    id_videojuego INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(10,2) NOT NULL,

    CONSTRAINT fk_detalle_venta
        FOREIGN KEY (id_venta)
        REFERENCES ventas(id_venta),

    CONSTRAINT fk_detalle_videojuego
        FOREIGN KEY (id_videojuego)
        REFERENCES videojuegos(id_videojuego)
);

INSERT INTO usuarios (nombre, usuario, contrasena)
VALUES
('Administrador', 'admin', '1234');

INSERT INTO videojuegos
(nombre, genero, plataforma, precio, stock)
VALUES
('Minecraft', 'Sandbox', 'PC', 29.99, 10),
('Grand Theft Auto V', 'Acción', 'PC', 29.99, 8),
('Resident Evil 4', 'Terror', 'PC', 39.99, 5),
('EA Sports FC 26', 'Deportes', 'PC', 69.99, 6);

SELECT * FROM usuarios;

SELECT * FROM videojuegos;

SELECT * FROM ventas;

SELECT * FROM detalle_venta;

## 🛠️ Tecnologías utilizadas

Para el desarrollo del sistema GameStore se utilizaron diferentes tecnologías y herramientas que permiten implementar la interfaz gráfica, la lógica del sistema y el almacenamiento de información.

| Tecnología / herramienta | Utilización                                                                                                                             |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Java**                 | Lenguaje principal utilizado para desarrollar la aplicación.                                                                            |
| **Apache NetBeans**      | Entorno de desarrollo utilizado para programar y diseñar las interfaces gráficas mediante JFrame.                                       |
| **Java Swing**           | Biblioteca utilizada para crear los componentes gráficos de la aplicación, como botones, etiquetas, campos de texto, tablas y ventanas. |
| **MySQL**                | Sistema gestor de base de datos utilizado para almacenar usuarios, videojuegos y ventas.                                                |
| **MySQL Connector/J**    | Controlador que permite establecer la comunicación entre Java y MySQL.                                                                  |
| **Git**                  | Sistema de control de versiones utilizado para registrar y administrar los cambios realizados en el código.                             |
| **GitHub**               | Plataforma utilizada para almacenar el repositorio del proyecto y facilitar el trabajo colaborativo entre los integrantes del grupo.    |
| **JDBC**                 | API utilizada para realizar la conexión y ejecutar operaciones sobre la base de datos MySQL desde Java.                                 |
| **Windows 11**           | Sistema operativo utilizado como entorno para el desarrollo y ejecución de la aplicación.                                               |

### Estructura del proyecto

El proyecto se organiza en diferentes paquetes para separar las responsabilidades del sistema:

* **Config:** contiene la configuración necesaria para establecer la conexión con MySQL.
* **DAO:** contiene las clases encargadas de realizar las operaciones de acceso a la base de datos, como insertar, consultar, modificar y eliminar información.
* **Modelo:** contiene las clases que representan las entidades de la base de datos, como usuarios, videojuegos y ventas.
* **Presentacion:** contiene las interfaces gráficas desarrolladas mediante JFrame y los eventos de los botones.
* **utilidades:** contiene funciones auxiliares utilizadas por diferentes partes de la aplicación.

Esta organización permite mantener separado el acceso a los datos, la lógica de las entidades y la interfaz gráfica, facilitando el mantenimiento y la modificación del sistema.


## 👩‍💻 Autores
Domenika Aumala,Reyes,Contreras,Olivo,PINOARGOTTY,Espinoza Ronquillo— Proyecto ABP / Examen Quimestral, Programación y Desarrollo de Base de Datos.
