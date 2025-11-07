## Modelo Fisico


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
