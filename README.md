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

# ejemplo de funcionamiento
https://i.ibb.co/N6CCntkx/Captura-desde-2025-11-07-17-00-44.png

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

-- ============================================
-- CONSULTAS 
-- ============================================

-- 1. Productos más vendidos
SELECT p.nombre, SUM(d.cantidad) AS total_vendidos
FROM detalle_pedidos d
JOIN productos p ON d.id_producto = p.id_producto
GROUP BY p.nombre
ORDER BY total_vendidos DESC;

-- 2. Total de ingresos generados por cada combo
SELECT c.nombre, c.precio, COUNT(cp.id_combo) AS veces_vendido, (c.precio * COUNT(cp.id_combo)) AS total_ingresos
FROM combos c
LEFT JOIN combo_producto cp ON c.id_combo = cp.id_combo
GROUP BY c.id_combo;

-- 3. Pedidos realizados para recoger vs. comer en la pizzería
SELECT tipo_pedido, COUNT(*) AS cantidad_pedidos
FROM pedidos
GROUP BY tipo_pedido;

-- 4. Adiciones más solicitadas
SELECT a.nombre, COUNT(pa.id_adicion) AS total
FROM producto_adicion pa
JOIN adicion a ON pa.id_adicion = a.id_adicion
GROUP BY a.nombre
ORDER BY total DESC;

-- 5. Cantidad total de productos vendidos por categoría
SELECT p.tipo, SUM(d.cantidad) AS total_vendidos
FROM detalle_pedidos d
JOIN productos p ON d.id_producto = p.id_producto
GROUP BY p.tipo;

-- 6. Promedio de pizzas pedidas por cliente
SELECT c.nombre, AVG(d.cantidad) AS promedio_pizzas
FROM pedidos pe
JOIN detalle_pedidos d ON pe.id_pedido = d.id_pedido
JOIN productos p ON d.id_producto = p.id_producto
JOIN clientes c ON pe.id_cliente = c.id_cliente
WHERE p.tipo = 'Pizza'
GROUP BY c.nombre;

-- 7. Total de ventas por día de la semana
SELECT DAYNAME(fecha) AS dia_semana, SUM(d.cantidad * d.precio_unitario) AS total_ventas
FROM pedidos p
JOIN detalle_pedidos d ON p.id_pedido = d.id_pedido
GROUP BY dia_semana;

-- 8. Cantidad de panzarottis vendidos con extra queso
SELECT COUNT(*) AS panzarottis_extra_queso
FROM detalle_pedidos dp
JOIN productos p ON dp.id_producto = p.id_producto
JOIN producto_adicion pa ON p.id_producto = pa.id_producto
JOIN adicion a ON pa.id_adicion = a.id_adicion
WHERE p.nombre LIKE '%Calzone%' AND a.nombre = 'Extra Queso';

-- 9. Pedidos que incluyen bebidas como parte de un combo
SELECT DISTINCT p.id_pedido
FROM detalle_pedidos d
JOIN productos pr ON d.id_producto = pr.id_producto
WHERE pr.tipo = 'Bebida';

-- 10. Clientes que han realizado más de 5 pedidos en el último mes
SELECT c.nombre, COUNT(p.id_pedido) AS total_pedidos
FROM pedidos p
JOIN clientes c ON p.id_cliente = c.id_cliente
WHERE p.fecha >= DATE_SUB(NOW(), INTERVAL 1 MONTH)
GROUP BY c.id_cliente
HAVING total_pedidos > 5;

-- 11. Ingresos totales por productos no elaborados (bebidas, postres, etc.)
SELECT SUM(d.cantidad * d.precio_unitario) AS total_ingresos_no_elaborados
FROM detalle_pedidos d
JOIN productos p ON d.id_producto = p.id_producto
WHERE p.tipo IN ('Bebida', 'Postre');

-- 12. Promedio de adiciones por pedido
SELECT AVG(total_adiciones) AS promedio_adiciones
FROM (
    SELECT p.id_pedido, COUNT(pa.id_adicion) AS total_adiciones
    FROM pedidos p
    JOIN detalle_pedidos dp ON p.id_pedido = dp.id_pedido
    JOIN producto_adicion pa ON dp.id_producto = pa.id_producto
    GROUP BY p.id_pedido
) AS sub;

-- 13. Total de combos vendidos en el último mes
SELECT COUNT(*) AS combos_vendidos_ultimo_mes
FROM combos c
JOIN combo_producto cp ON c.id_combo = cp.id_combo
WHERE NOW() >= DATE_SUB(NOW(), INTERVAL 1 MONTH);

-- 14. Clientes con pedidos para recoger y en tienda
SELECT c.nombre
FROM pedidos p
JOIN clientes c ON p.id_cliente = c.id_cliente
GROUP BY c.id_cliente
HAVING SUM(p.tipo_pedido = 'Domicilio') > 0 AND SUM(p.tipo_pedido = 'En tienda') > 0;

-- 15. Total de productos personalizados con adiciones
SELECT COUNT(DISTINCT pa.id_producto) AS productos_personalizados
FROM producto_adicion pa;

-- 16. Pedidos con más de 3 productos diferentes
SELECT p.id_pedido, COUNT(DISTINCT d.id_producto) AS total_productos
FROM pedidos p
JOIN detalle_pedidos d ON p.id_pedido = d.id_pedido
GROUP BY p.id_pedido
HAVING total_productos > 3;

-- 17. Promedio de ingresos generados por día
SELECT AVG(total_dia) AS promedio_diario
FROM (
    SELECT DATE(p.fecha) AS dia, SUM(d.cantidad * d.precio_unitario) AS total_dia
    FROM pedidos p
    JOIN detalle_pedidos d ON p.id_pedido = d.id_pedido
    GROUP BY dia
) AS sub;

-- 18. Clientes que han pedido pizzas con adiciones en más del 50% de sus pedidos
SELECT c.nombre
FROM clientes c
JOIN pedidos p ON c.id_cliente = p.id_cliente
JOIN detalle_pedidos d ON p.id_pedido = d.id_pedido
JOIN productos pr ON d.id_producto = pr.id_producto
JOIN producto_adicion pa ON pr.id_producto = pa.id_producto
WHERE pr.tipo = 'Pizza'
GROUP BY c.id_cliente
HAVING (COUNT(DISTINCT p.id_pedido) / (SELECT COUNT(*) FROM pedidos WHERE id_cliente = c.id_cliente)) > 0.5;

-- 19. Porcentaje de ventas provenientes de productos no elaborados
SELECT 
    (SUM(CASE WHEN p.tipo IN ('Bebida', 'Postre') THEN d.cantidad * d.precio_unitario ELSE 0 END) / 
     SUM(d.cantidad * d.precio_unitario)) * 100 AS porcentaje_no_elaborados
FROM detalle_pedidos d
JOIN productos p ON d.id_producto = p.id_producto;

-- 20. Día de la semana con mayor número de pedidos para recoger
SELECT DAYNAME(fecha) AS dia_semana, COUNT(*) AS cantidad
FROM pedidos
WHERE tipo_pedido = 'Domicilio'
GROUP BY dia_semana
ORDER BY cantidad DESC
LIMIT 1;






