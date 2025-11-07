# Examen_Mysql-I

El propósito de este examen es diseñar una base de datos que permita gestionar eficientemente los productos, combos, pedidos y clientes de una pizzería. El sistema debe almacenar información sobre pizzas, panzarottis, otros productos no elaborados (bebidas, postres, etc.), adiciones y el menú disponible. Además, se deberán registrar los pedidos de los clientes, que pueden ser para consumir en el lugar o para recoger.



# Problema


La pizzería actualmente no tiene un sistema centralizado para gestionar sus productos y pedidos, lo que genera confusión al manejar inventarios, combos y opciones de pedidos. También resulta difícil hacer un seguimiento de los productos más vendidos o personalizar pedidos. Por lo tanto, se requiere un sistema que permita gestionar de manera eficiente el menú, las combinaciones de productos, las ventas, y los pedidos.



# Características Principales
Gestión de Productos: Registro completo de pizzas, panzarottis, bebidas, postres y otros productos no elaborados. Se debe tener en cuenta los ingredientes que poseen los productos.
Gestión de Adiciones: Permitir a los clientes agregar adiciones (extra queso, salsas, etc.) a sus productos.
Gestión de Combos: Manejar combos que incluyen múltiples productos a un precio especial.
Gestión de Pedidos: Registro de pedidos para consumir en la pizzería o para recoger, con opción de personalización mediante adiciones.
Gestión del Menú: Definir y actualizar el menú con las opciones disponibles.


# Requisitos del Modelo Lógico y Físico
El modelo lógico debe reflejar correctamente las entidades, relaciones, atributos y cardinalidades, cubriendo todas las características principales.
El modelo físico debe ser implementado en una base de datos MySQL, incluyendo las tablas necesarias con sus claves primarias y foráneas.
Evidencia fotográfica o uso de plataformas como drawSQL o StarUML debe ser proporcionada, ya sea en forma de capturas de pantalla o enlaces a los diagramas (No se reciben archivos para abrir en ningún software).


# Tecnologías y Herramientas
Base de Datos: MySQL para gestionar la información.
Lenguaje de Consulta: SQL para las consultas necesarias.
Herramientas de Diseño: MySQL Workbench u otras herramientas de diseño de bases de datos.


# Solucion
# Modelo Logico
https://i.ibb.co/8Z0Grs5/Captura-desde-2025-11-07-15-53-48.png
# Codigo Mysql

DROP DATABASE IF EXISTS campuslands_pizzas;
CREATE DATABASE campuslands_pizzas;
USE campuslands_pizzas;


CREATE TABLE clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    direccion VARCHAR(255),
    telefono VARCHAR(20),
    correo VARCHAR(100)
) ENGINE=InnoDB;


CREATE TABLE productos (
    id_producto INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    tipo  VARCHAR(255),
    precio DECIMAL(10, 2) NOT NULL,
    descripcion VARCHAR(255)
) ENGINE=InnoDB;


CREATE TABLE ingredientes (
    id_ingrediente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    descripcion VARCHAR(255)
) ENGINE=InnoDB;


CREATE TABLE adicion (
    id_adicion INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    precio_extra DECIMAL(11, 1)
) ENGINE=InnoDB;


CREATE TABLE combos (
    id_combo INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    descripcion VARCHAR(255),
    precio DECIMAL(10, 2)
) ENGINE=InnoDB;


CREATE TABLE pedidos (
    id_pedido INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    fecha DATETIME,
    tipo_pedido VARCHAR(255),
    estado VARCHAR(255),
    FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
) ENGINE=InnoDB;


CREATE TABLE detalle_pedidos (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_pedido INT,
    id_producto INT,
    cantidad INT,
    precio_unitario DECIMAL(10, 2),
    FOREIGN KEY (id_pedido) REFERENCES pedidos(id_pedido),
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto)
) ENGINE=InnoDB;


CREATE TABLE producto_adicion (
    id_producto INT,
    id_adicion INT,
    PRIMARY KEY (id_producto, id_adicion),
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto),
    FOREIGN KEY (id_adicion) REFERENCES adicion(id_adicion)
) ENGINE=InnoDB;


CREATE TABLE combo_producto (
    id_combo INT,
    id_producto INT,
    PRIMARY KEY (id_combo, id_producto),
    FOREIGN KEY (id_combo) REFERENCES combos(id_combo),
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto)
) ENGINE=InnoDB;





INSERT INTO clientes (nombre, direccion, telefono, correo) VALUES
('Juan Pérez', 'Calle 10 #45-20', '3001234567', 'juanp@gmail.com'),
('María López', 'Carrera 7 #15-80', '3019876543', 'maria.l@gmail.com'),
('Carlos Díaz', 'Av. Siempre Viva 123', '3024567890', 'carlosd@gmail.com'),
('Ana Torres', 'Calle 20 #30-55', '3049988776', 'ana.torres@gmail.com'),
('Luis Gómez', 'Carrera 50 #12-34', '3056677889', 'luis.gomez@gmail.com');


INSERT INTO productos (nombre, tipo, precio, descripcion) VALUES
('Pizza Margarita', 'Pizza', 28000, 'Tomate, mozzarella y albahaca fresca'),
('Pizza Hawaiana', 'Pizza', 30000, 'Jamón, piña y queso mozzarella'),
('Pizza Pepperoni', 'Pizza', 32000, 'Pepperoni y queso mozzarella'),
('Lasagna de Carne', 'Pasta', 25000, 'Carne molida, salsa boloñesa y queso'),
('Pan de Ajo', 'Entrada', 8000, 'Pan horneado con mantequilla y ajo'),
('Bebida Gaseosa', 'Bebida', 5000, 'Gaseosa de 400ml'),
('Jugo Natural', 'Bebida', 7000, 'Jugo de fruta natural'),
('Pizza Vegetariana', 'Pizza', 31000, 'Pimientos, champiñones y cebolla'),
('Calzone', 'Pizza', 27000, 'Masa rellena de jamón y queso'),
('Ensalada César', 'Ensalada', 18000, 'Lechuga, pollo y aderezo césar');


INSERT INTO ingredientes (nombre, descripcion) VALUES
('Queso Mozzarella', 'Queso fundido'),
('Salsa de Tomate', 'Base de pizza'),
('Jamón', 'Jamón de cerdo'),
('Piña', 'Piña en trozos'),
('Pepperoni', 'Rodajas de salami picante'),
('Tocineta', 'Tiras de tocineta frita'),
('Champiñones', 'Hongos frescos laminados'),
('Cebolla', 'Cebolla blanca en rodajas'),
('Albahaca', 'Hojas frescas de albahaca'),
('Aceitunas', 'Aceitunas negras sin hueso');


INSERT INTO adicion (nombre, precio_extra) VALUES
('Extra Queso', 3000),
('Tocineta', 3500),
('Aceitunas', 2000),
('Champiñones', 2500),
('Jamón Extra', 3000);


INSERT INTO combos (nombre, descripcion, precio) VALUES
('Combo Familiar', '2 pizzas grandes + 1 bebida 1.5L', 65000),
('Combo Pareja', '1 pizza mediana + 2 bebidas personales', 40000),
('Combo Individual', '1 porción de pizza + 1 bebida', 18000);


INSERT INTO combo_producto (id_combo, id_producto) VALUES
(1, 1), (1, 2), (1, 6),
(2, 3), (2, 6),
(3, 1), (3, 6);


INSERT INTO pedidos (id_cliente, fecha, tipo_pedido, estado) VALUES
(1, NOW(), 'Domicilio', 'Entregado'),
(2, NOW(), 'En tienda', 'Pendiente'),
(3, NOW(), 'Domicilio', 'En camino'),
(4, NOW(), 'Domicilio', 'Entregado'),
(5, NOW(), 'En tienda', 'Cancelado');


INSERT INTO detalle_pedidos (id_pedido, id_producto, cantidad, precio_unitario) VALUES
(1, 1, 1, 28000),
(1, 6, 2, 5000),
(2, 2, 1, 30000),
(3, 3, 2, 32000),
(4, 4, 1, 25000),
(4, 6, 1, 5000),
(5, 5, 1, 8000);

INSERT INTO producto_adicion (id_producto, id_adicion) VALUES
(1, 1),
(1, 3),
(2, 2),
(3, 1),
(3, 4),
(8, 5);



