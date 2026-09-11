# 🎮 GameStore - Sistema de Gestión de Venta de Videojuegos

## 📌 Contexto

**GameStore** es una aplicación de escritorio desarrollada en Java con conexión a una base de datos MySQL, orientada a resolver la gestión de una tienda dedicada a la venta de videojuegos físicos y digitales (títulos como *League of Legends*, *Call of Duty*, *Fortnite*, *Minecraft*, *GTA VI*, *Super Mario*, *Mortal Kombat*, *Brawl Stars*, *Among Us*, *Poppy Playtime* y *Stumble Guys*, entre otros).

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

```sql
CREATE DATABASE IF NOT EXISTS gamezone_db;
USE gamezone_db;

CREATE TABLE categorias (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nombre_categoria VARCHAR(50) NOT NULL
);

CREATE TABLE videojuegos (
    id_videojuego INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    plataforma VARCHAR(50) NOT NULL,
    precio DECIMAL(10,2) NOT NULL,
    stock INT NOT NULL DEFAULT 0,
    id_categoria INT,
    FOREIGN KEY (id_categoria) REFERENCES categorias(id_categoria)
);

CREATE TABLE clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    cedula VARCHAR(10) UNIQUE NOT NULL,
    telefono VARCHAR(15),
    email VARCHAR(100)
);

CREATE TABLE ventas (
    id_venta INT AUTO_INCREMENT PRIMARY KEY,
    fecha DATE NOT NULL,
    id_cliente INT,
    total DECIMAL(10,2) NOT NULL DEFAULT 0,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
);

CREATE TABLE detalle_venta (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_venta INT,
    id_videojuego INT,
    cantidad INT NOT NULL,
    subtotal DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (id_venta) REFERENCES ventas(id_venta),
    FOREIGN KEY (id_videojuego) REFERENCES videojuegos(id_videojuego)
);

-- Datos de ejemplo
INSERT INTO categorias (nombre_categoria) VALUES
('Battle Royale'), ('Aventura'), ('Plataformas'), ('Lucha'), ('Sandbox'), ('Multijugador');

INSERT INTO videojuegos (nombre, plataforma, precio, stock, id_categoria) VALUES
('Fortnite', 'Multiplataforma', 0.00, 100, 1),
('Call of Duty', 'PS5/Xbox/PC', 59.99, 30, 6),
('League of Legends', 'PC', 0.00, 100, 6),
('Minecraft', 'Multiplataforma', 26.95, 50, 5),
('Grand Theft Auto VI', 'PS5/Xbox', 69.99, 20, 2),
('Super Mario', 'Nintendo Switch', 49.99, 25, 3),
('Mortal Kombat', 'PS5/Xbox/PC', 54.99, 15, 4),
('Brawl Stars', 'Móvil', 0.00, 100, 6),
('Among Us', 'Multiplataforma', 4.99, 100, 6),
('Poppy Playtime', 'PC', 9.99, 40, 2),
('Stumble Guys', 'Móvil', 0.00, 100, 6);
```


## 🛠️ Tecnologías utilizadas
- Java (Swing / JFrame)
- MySQL
- JDBC (Java Database Connectivity)

## 👩‍💻 Autores
Domenika Aumala,Reyes,Contreras,Olivo,PINOARGOTTY,Espinoza Ronquillo— Proyecto ABP / Examen Quimestral, Programación y Desarrollo de Base de Datos.
