# Crear la base de datos TiendaDB

## Script para generar la base de datos

### Sql Server

```sql:
CREATE TABLE categoria (
    idcategoria INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE,
    descripcion VARCHAR(255),
    estado BIT DEFAULT 1 CHECK (estado IN (0, 1))
);

CREATE TABLE articulo (
    idarticulo INT IDENTITY(1,1) PRIMARY KEY,
    idcategoria INT NOT NULL,
    codigo VARCHAR(50) UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    precio_venta DECIMAL(11, 2) CHECK (precio_venta >= 0),
    stock INT CHECK (stock >= 0),
    descripcion VARCHAR(255),
    imagen VARCHAR(100),
    estado BIT DEFAULT 1 CHECK (estado IN (0, 1)),
    FOREIGN KEY (idcategoria) REFERENCES categoria(idcategoria)
);

CREATE TABLE persona (
    idpersona INT IDENTITY(1,1) PRIMARY KEY,
    tipo_persona VARCHAR(20) NOT NULL CHECK (tipo_persona IN ('Cliente', 'Proveedor')),
    nombre VARCHAR(100) NOT NULL,
    tipo_documento VARCHAR(20),
    num_documento VARCHAR(20) UNIQUE,
    direccion VARCHAR(70),
    telefono VARCHAR(20),
    email VARCHAR(50) UNIQUE
);

CREATE TABLE usuario (
    idusuario INT IDENTITY(1,1) PRIMARY KEY,
    idrol INT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    tipo_documento VARCHAR(20),
    num_documento VARCHAR(20) UNIQUE,
    direccion VARCHAR(70),
    telefono VARCHAR(20),
    email VARCHAR(50) UNIQUE,
    clave VARBINARY(MAX) NOT NULL,
    estado BIT DEFAULT 1 CHECK (estado IN (0, 1)),
    FOREIGN KEY (idrol) REFERENCES rol(idrol)
);

CREATE TABLE rol (
    idrol INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(30) NOT NULL UNIQUE,
    descripcion VARCHAR(255),
    estado BIT DEFAULT 1 CHECK (estado IN (0, 1))
);

CREATE TABLE ingreso (
    idingreso INT IDENTITY(1,1) PRIMARY KEY,
    idproveedor INT NOT NULL,
    idusuario INT NOT NULL,
    tipo_comprobante VARCHAR(20),
    serie_comprobante VARCHAR(7),
    num_comprobante VARCHAR(10) UNIQUE,
    fecha DATETIME DEFAULT GETDATE(),
    impuesto DECIMAL(4, 2) CHECK (impuesto >= 0),
    total DECIMAL(11, 2) CHECK (total >= 0),
    estado VARCHAR(20) DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idproveedor) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_ingreso (
    iddetalle_ingreso INT IDENTITY(1,1) PRIMARY KEY,
    idingreso INT NOT NULL,
    idarticulo INT NOT NULL,
    cantidad INT CHECK (cantidad > 0),
    precio DECIMAL(11, 2) CHECK (precio >= 0),
    FOREIGN KEY (idingreso) REFERENCES ingreso(idingreso),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);

CREATE TABLE venta (
    idventa INT IDENTITY(1,1) PRIMARY KEY,
    idcliente INT NOT NULL,
    idusuario INT NOT NULL,
    tipo_comprobante VARCHAR(20),
    serie_comprobante VARCHAR(7),
    num_comprobante VARCHAR(10) UNIQUE,
    fecha DATETIME DEFAULT GETDATE(),
    impuesto DECIMAL(4, 2) CHECK (impuesto >= 0),
    total DECIMAL(11, 2) CHECK (total >= 0),
    estado VARCHAR(20) DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idcliente) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_venta (
    iddetalle_venta INT IDENTITY(1,1) PRIMARY KEY,
    idventa INT NOT NULL,
    idarticulo INT NOT NULL,
    cantidad INT CHECK (cantidad > 0),
    precio DECIMAL(11, 2) CHECK (precio >= 0),
    descuento DECIMAL(11, 2) CHECK (descuento >= 0),
    FOREIGN KEY (idventa) REFERENCES venta(idventa),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);
```

### MariaDB

```sql:
CREATE TABLE categoria (
    idcategoria INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE,
    descripcion VARCHAR(255),
    estado TINYINT(1) DEFAULT 1 CHECK (estado IN (0, 1))
);

CREATE TABLE articulo (
    idarticulo INT AUTO_INCREMENT PRIMARY KEY,
    idcategoria INT NOT NULL,
    codigo VARCHAR(50) UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    precio_venta DECIMAL(11, 2) CHECK (precio_venta >= 0),
    stock INT CHECK (stock >= 0),
    descripcion VARCHAR(255),
    imagen VARCHAR(100),
    estado TINYINT(1) DEFAULT 1 CHECK (estado IN (0, 1)),
    FOREIGN KEY (idcategoria) REFERENCES categoria(idcategoria)
);

CREATE TABLE persona (
    idpersona INT AUTO_INCREMENT PRIMARY KEY,
    tipo_persona VARCHAR(20) NOT NULL CHECK (tipo_persona IN ('Cliente', 'Proveedor')),
    nombre VARCHAR(100) NOT NULL,
    tipo_documento VARCHAR(20),
    num_documento VARCHAR(20) UNIQUE,
    direccion VARCHAR(70),
    telefono VARCHAR(20),
    email VARCHAR(50) UNIQUE
);

CREATE TABLE usuario (
    idusuario INT AUTO_INCREMENT PRIMARY KEY,
    idrol INT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    tipo_documento VARCHAR(20),
    num_documento VARCHAR(20) UNIQUE,
    direccion VARCHAR(70),
    telefono VARCHAR(20),
    email VARCHAR(50) UNIQUE,
    clave VARBINARY(255) NOT NULL,
    estado TINYINT(1) DEFAULT 1 CHECK (estado IN (0, 1)),
    FOREIGN KEY (idrol) REFERENCES rol(idrol)
);

CREATE TABLE rol (
    idrol INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(30) NOT NULL UNIQUE,
    descripcion VARCHAR(255),
    estado TINYINT(1) DEFAULT 1 CHECK (estado IN (0, 1))
);

CREATE TABLE ingreso (
    idingreso INT AUTO_INCREMENT PRIMARY KEY,
    idproveedor INT NOT NULL,
    idusuario INT NOT NULL,
    tipo_comprobante VARCHAR(20),
    serie_comprobante VARCHAR(7),
    num_comprobante VARCHAR(10) UNIQUE,
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
    impuesto DECIMAL(4, 2) CHECK (impuesto >= 0),
    total DECIMAL(11, 2) CHECK (total >= 0),
    estado VARCHAR(20) DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idproveedor) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_ingreso (
    iddetalle_ingreso INT AUTO_INCREMENT PRIMARY KEY,
    idingreso INT NOT NULL,
    idarticulo INT NOT NULL,
    cantidad INT CHECK (cantidad > 0),
    precio DECIMAL(11, 2) CHECK (precio >= 0),
    FOREIGN KEY (idingreso) REFERENCES ingreso(idingreso),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);

CREATE TABLE venta (
    idventa INT AUTO_INCREMENT PRIMARY KEY,
    idcliente INT NOT NULL,
    idusuario INT NOT NULL,
    tipo_comprobante VARCHAR(20),
    serie_comprobante VARCHAR(7),
    num_comprobante VARCHAR(10) UNIQUE,
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
    impuesto DECIMAL(4, 2) CHECK (impuesto >= 0),
    total DECIMAL(11, 2) CHECK (total >= 0),
    estado VARCHAR(20) DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idcliente) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_venta (
    iddetalle_venta INT AUTO_INCREMENT PRIMARY KEY,
    idventa INT NOT NULL,
    idarticulo INT NOT NULL,
    cantidad INT CHECK (cantidad > 0),
    precio DECIMAL(11, 2) CHECK (precio >= 0),
    descuento DECIMAL(11, 2) CHECK (descuento >= 0),
    FOREIGN KEY (idventa) REFERENCES venta(idventa),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);
```

### SQlite

```sql:
CREATE TABLE categoria (
    idcategoria INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL UNIQUE,
    descripcion TEXT,
    estado INTEGER DEFAULT 1 CHECK (estado IN (0, 1))
);

CREATE TABLE articulo (
    idarticulo INTEGER PRIMARY KEY AUTOINCREMENT,
    idcategoria INTEGER NOT NULL,
    codigo TEXT UNIQUE,
    nombre TEXT NOT NULL,
    precio_venta REAL CHECK (precio_venta >= 0),
    stock INTEGER CHECK (stock >= 0),
    descripcion TEXT,
    imagen TEXT,
    estado INTEGER DEFAULT 1 CHECK (estado IN (0, 1)),
    FOREIGN KEY (idcategoria) REFERENCES categoria(idcategoria)
);

CREATE TABLE persona (
    idpersona INTEGER PRIMARY KEY AUTOINCREMENT,
    tipo_persona TEXT NOT NULL CHECK (tipo_persona IN ('Cliente', 'Proveedor')),
    nombre TEXT NOT NULL,
    tipo_documento TEXT,
    num_documento TEXT UNIQUE,
    direccion TEXT,
    telefono TEXT,
    email TEXT UNIQUE
);

CREATE TABLE usuario (
    idusuario INTEGER PRIMARY KEY AUTOINCREMENT,
    idrol INTEGER NOT NULL,
    nombre TEXT NOT NULL,
    tipo_documento TEXT,
    num_documento TEXT UNIQUE,
    direccion TEXT,
    telefono TEXT,
    email TEXT UNIQUE,
    clave BLOB NOT NULL,
    estado INTEGER DEFAULT 1 CHECK (estado IN (0, 1)),
    FOREIGN KEY (idrol) REFERENCES rol(idrol)
);

CREATE TABLE rol (
    idrol INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL UNIQUE,
    descripcion TEXT,
    estado INTEGER DEFAULT 1 CHECK (estado IN (0, 1))
);

CREATE TABLE ingreso (
    idingreso INTEGER PRIMARY KEY AUTOINCREMENT,
    idproveedor INTEGER NOT NULL,
    idusuario INTEGER NOT NULL,
    tipo_comprobante TEXT,
    serie_comprobante TEXT,
    num_comprobante TEXT UNIQUE,
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
    impuesto REAL CHECK (impuesto >= 0),
    total REAL CHECK (total >= 0),
    estado TEXT DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idproveedor) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_ingreso (
    iddetalle_ingreso INTEGER PRIMARY KEY AUTOINCREMENT,
    idingreso INTEGER NOT NULL,
    idarticulo INTEGER NOT NULL,
    cantidad INTEGER CHECK (cantidad > 0),
    precio REAL CHECK (precio >= 0),
    FOREIGN KEY (idingreso) REFERENCES ingreso(idingreso),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);

CREATE TABLE venta (
    idventa INTEGER PRIMARY KEY AUTOINCREMENT,
    idcliente INTEGER NOT NULL,
    idusuario INTEGER NOT NULL,
    tipo_comprobante TEXT,
    serie_comprobante TEXT,
    num_comprobante TEXT UNIQUE,
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
    impuesto REAL CHECK (impuesto >= 0),
    total REAL CHECK (total >= 0),
    estado TEXT DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idcliente) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_venta (
    iddetalle_venta INTEGER PRIMARY KEY AUTOINCREMENT,
    idventa INTEGER NOT NULL,
    idarticulo INTEGER NOT NULL,
    cantidad INTEGER CHECK (cantidad > 0),
    precio REAL CHECK (precio >= 0),
    descuento REAL CHECK (descuento >= 0),
    FOREIGN KEY (idventa) REFERENCES venta(idventa),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);
```

### PostgreSQL

```sql:
CREATE TABLE categoria (
    idcategoria SERIAL PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE,
    descripcion VARCHAR(255),
    estado BOOLEAN DEFAULT TRUE CHECK (estado IN (FALSE, TRUE))
);

CREATE TABLE articulo (
    idarticulo SERIAL PRIMARY KEY,
    idcategoria INT NOT NULL,
    codigo VARCHAR(50) UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    precio_venta NUMERIC(11, 2) CHECK (precio_venta >= 0),
    stock INT CHECK (stock >= 0),
    descripcion VARCHAR(255),
    imagen VARCHAR(100),
    estado BOOLEAN DEFAULT TRUE CHECK (estado IN (FALSE, TRUE)),
    FOREIGN KEY (idcategoria) REFERENCES categoria(idcategoria)
);

CREATE TABLE persona (
    idpersona SERIAL PRIMARY KEY,
    tipo_persona VARCHAR(20) NOT NULL CHECK (tipo_persona IN ('Cliente', 'Proveedor')),
    nombre VARCHAR(100) NOT NULL,
    tipo_documento VARCHAR(20),
    num_documento VARCHAR(20) UNIQUE,
    direccion VARCHAR(70),
    telefono VARCHAR(20),
    email VARCHAR(50) UNIQUE
);

CREATE TABLE usuario (
    idusuario SERIAL PRIMARY KEY,
    idrol INT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    tipo_documento VARCHAR(20),
    num_documento VARCHAR(20) UNIQUE,
    direccion VARCHAR(70),
    telefono VARCHAR(20),
    email VARCHAR(50) UNIQUE,
    clave BYTEA NOT NULL,
    estado BOOLEAN DEFAULT TRUE CHECK (estado IN (FALSE, TRUE)),
    FOREIGN KEY (idrol) REFERENCES rol(idrol)
);

CREATE TABLE rol (
    idrol SERIAL PRIMARY KEY,
    nombre VARCHAR(30) NOT NULL UNIQUE,
    descripcion VARCHAR(255),
    estado BOOLEAN DEFAULT TRUE CHECK (estado IN (FALSE, TRUE))
);

CREATE TABLE ingreso (
    idingreso SERIAL PRIMARY KEY,
    idproveedor INT NOT NULL,
    idusuario INT NOT NULL,
    tipo_comprobante VARCHAR(20),
    serie_comprobante VARCHAR(7),
    num_comprobante VARCHAR(10) UNIQUE,
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    impuesto NUMERIC(4, 2) CHECK (impuesto >= 0),
    total NUMERIC(11, 2) CHECK (total >= 0),
    estado VARCHAR(20) DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idproveedor) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_ingreso (
    iddetalle_ingreso SERIAL PRIMARY KEY,
    idingreso INT NOT NULL,
    idarticulo INT NOT NULL,
    cantidad INT CHECK (cantidad > 0),
    precio NUMERIC(11, 2) CHECK (precio >= 0),
    FOREIGN KEY (idingreso) REFERENCES ingreso(idingreso),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);

CREATE TABLE venta (
    idventa SERIAL PRIMARY KEY,
    idcliente INT NOT NULL,
    idusuario INT NOT NULL,
    tipo_comprobante VARCHAR(20),
    serie_comprobante VARCHAR(7),
    num_comprobante VARCHAR(10) UNIQUE,
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    impuesto NUMERIC(4, 2) CHECK (impuesto >= 0),
    total NUMERIC(11, 2) CHECK (total >= 0),
    estado VARCHAR(20) DEFAULT 'Pendiente' CHECK (estado IN ('Pendiente', 'Completado', 'Anulado')),
    FOREIGN KEY (idcliente) REFERENCES persona(idpersona),
    FOREIGN KEY (idusuario) REFERENCES usuario(idusuario)
);

CREATE TABLE detalle_venta (
    iddetalle_venta SERIAL PRIMARY KEY,
    idventa INT NOT NULL,
    idarticulo INT NOT NULL,
    cantidad INT CHECK (cantidad > 0),
    precio NUMERIC(11, 2) CHECK (precio >= 0),
    descuento NUMERIC(11, 2) CHECK (descuento >= 0),
    FOREIGN KEY (idventa) REFERENCES venta(idventa),
    FOREIGN KEY (idarticulo) REFERENCES articulo(idarticulo)
);
```

## Script para insertar los registros

```sql:
-- Insertar registros en la tabla rol
INSERT INTO rol (nombre, descripcion, estado) VALUES
('Administrador', 'Administrador del sistema', 1),
('Vendedor', 'Vendedor de productos', 1),
('Cliente', 'Cliente de la tienda', 1),
('Gerente', 'Gerente de la tienda', 1),
('Soporte', 'Soporte técnico', 1),
('Contador', 'Contador de la tienda', 1),
('Analista', 'Analista de datos', 1),
('Desarrollador', 'Desarrollador de sistemas', 1),
('Consultor', 'Consultor en tecnología', 1),
('Ingeniero', 'Ingeniero de sistemas', 1);

-- Insertar registros en la tabla categoria
INSERT INTO categoria (nombre, descripcion, estado) VALUES
('Computadoras de Escritorio', 'Computadoras de escritorio y torres', 1),
('Laptops', 'Laptops y portátiles', 1),
('Periféricos', 'Teclados, ratones, monitores, etc.', 1),
('Componentes', 'Componentes para ensamblar computadoras', 1),
('Software', 'Software y sistemas operativos', 1),
('Accesorios', 'Accesorios para computadoras', 1),
('Impresoras', 'Impresoras y multifuncionales', 1),
('Almacenamiento', 'Dispositivos de almacenamiento', 1),
('Redes', 'Equipos de red y accesorios', 1),
('Consumibles', 'Consumibles de computación', 1);

-- Insertar registros en la tabla articulo
INSERT INTO articulo (idcategoria, codigo, nombre, precio_venta, stock, descripcion, imagen, estado) VALUES
(1, 'PC001', 'Computadora de Escritorio HP', 899.99, 20, 'PC de escritorio con procesador Intel Core i5 y 8GB de RAM', 'pc_hp.jpg', 1),
(1, 'PC002', 'Computadora de Escritorio Dell', 799.99, 15, 'PC de escritorio con procesador AMD Ryzen y 12GB de RAM', 'pc_dell.jpg', 1),
(2, 'LAP001', 'Laptop Lenovo ThinkPad', 1099.99, 10, 'Laptop profesional con procesador Intel Core i7 y 16GB de RAM', 'laptop_lenovo.jpg', 1),
(2, 'LAP002', 'Laptop ASUS VivoBook', 899.99, 12, 'Laptop delgada y ligera con procesador AMD Ryzen y 8GB de RAM', 'laptop_asus.jpg', 1),
(3, 'TECL001', 'Teclado mecánico Logitech', 69.99, 30, 'Teclado mecánico con retroiluminación RGB', 'teclado_logitech.jpg', 1),
(3, 'RAT001', 'Ratón inalámbrico Microsoft', 39.99, 40, 'Ratón inalámbrico con sensor óptico de alta precisión', 'raton_microsoft.jpg', 1),
(4, 'RAM001', 'Memoria RAM Corsair', 79.99, 50, 'Memoria RAM DDR4 de 16GB para mejorar el rendimiento de tu computadora', 'ram_corsair.jpg', 1),
(4, 'SSD001', 'Disco de Estado Sólido Samsung', 129.99, 25, 'SSD de 500GB con velocidades de lectura/escritura rápidas', 'ssd_samsung.jpg', 1),
(5, 'WIN001', 'Windows 10 Pro', 199.99, 100, 'Sistema operativo Windows 10 Pro original', 'windows_10.jpg', 1),
(5, 'OFF001', 'Microsoft Office 365', 149.99, 80, 'Suite de productividad con Word, Excel, PowerPoint, etc.', 'office_365.jpg', 1),
(6, 'MOU001', 'Mousepad SteelSeries', 19.99, 50, 'Mousepad de tela con base de goma antideslizante', 'mousepad_steelseries.jpg', 1),
(6, 'AUR001', 'Audífonos Razer', 99.99, 30, 'Audífonos con sonido envolvente y micrófono retráctil', 'audifonos_razer.jpg', 1),
(7, 'IMP001', 'Impresora HP DeskJet', 129.99, 20, 'Impresora multifuncional de inyección de tinta', 'impresora_hp.jpg', 1),
(7, 'IMP002', 'Impresora Epson EcoTank', 299.99, 15, 'Impresora multifuncional con tanque de tinta recargable', 'impresora_epson.jpg', 1),
(8, 'HDD001', 'Disco Duro Externo WD', 79.99, 35, 'Disco duro externo portátil de 1TB', 'hdd_wd.jpg', 1),
(8, 'USB001', 'Memoria USB Kingston', 14.99, 100, 'Memoria USB de 32GB con conectividad USB 3.0', 'usb_kingston.jpg', 1),
(9, 'ROUT001', 'Router TP-Link', 49.99, 25, 'Router inalámbrico de doble banda', 'router_tplink.jpg', 1),
(9, 'SWITCH001', 'Switch Cisco', 199.99, 10, 'Switch de red de 24 puertos Gigabit Ethernet', 'switch_cisco.jpg', 1),
(10, 'TONER001', 'Tóner HP', 59.99, 50, 'Cartucho de tóner original para impresoras HP', 'toner_hp.jpg', 1),
(10, 'PAPEL001', 'Papel para Impresora', 9.99, 200, 'Paquete de papel A4 para impresoras láser y de inyección de tinta', 'papel_impresora.jpg', 1);

-- Insertar registros en la tabla persona
INSERT INTO persona (tipo_persona, nombre, tipo_documento, num_documento, direccion, telefono, email) VALUES
('Cliente', 'Ana López', 'DNI', '12345678', 'Av. Principal 123', '555-1234', 'ana.lopez@example.com'),
('Cliente', 'Carlos Pérez', 'DNI', '23456789', 'Calle Secundaria 456', '555-2345', 'carlos.perez@example.com'),
('Cliente', 'María Rodríguez', 'DNI', '34567890', 'Av. Independencia 789', '555-3456', 'maria.rodriguez@example.com'),
('Cliente', 'Juan Martínez', 'DNI', '45678901', 'Calle Central 012', '555-4567', 'juan.martinez@example.com'),
('Cliente', 'Laura Gómez', 'DNI', '56789012', 'Av. Libertad 345', '555-5678', 'laura.gomez@example.com'),
('Cliente', 'Diego Suárez', 'DNI', '67890123', 'Calle Mayor 678', '555-6789', 'diego.suarez@example.com'),
('Cliente', 'Sofía Vásquez', 'DNI', '78901234', 'Av. Real 901', '555-7890', 'sofia.vasquez@example.com'),
('Cliente', 'Pedro Jiménez', 'DNI', '89012345', 'Calle Ficticia 234', '555-8901', 'pedro.jimenez@example.com'),
('Cliente', 'Lucía Torres', 'DNI', '90123456', 'Av. Imaginaria 567', '555-9012', 'lucia.torres@example.com'),
('Cliente', 'Gabriel Díaz', 'DNI', '01234567', 'Calle Irreal 890', '555-0123', 'gabriel.diaz@example.com'),
('Cliente', 'Elena Sánchez', 'DNI', '09876543', 'Av. Virtual 678', '555-0987', 'elena.sanchez@example.com'),
('Cliente', 'Javier Castro', 'DNI', '98765432', 'Calle Digital 901', '555-9876', 'javier.castro@example.com');

-- Insertar registros en la tabla usuario
INSERT INTO usuario (idrol, nombre, tipo_documento, num_documento, direccion, telefono, email, clave, estado) VALUES
(1, 'Admin', 'DNI', '00000000', 'Av. Administrativa 001', '555-0000', 'admin@example.com', CONVERT(VARBINARY(MAX), 'adminpass'), 1),
(2, 'Vendedor 1', 'DNI', '11111111', 'Av. Vendedor 001', '555-1111', 'vendedor1@example.com', CONVERT(VARBINARY(MAX), 'vendedor1pass'), 1),
(2, 'Vendedor 2', 'DNI', '22222222', 'Av. Vendedor 002', '555-2222', 'vendedor2@example.com', CONVERT(VARBINARY(MAX), 'vendedor2pass'), 1),
(2, 'Vendedor 3', 'DNI', '33333333', 'Av. Vendedor 003', '555-3333', 'vendedor3@example.com', CONVERT(VARBINARY(MAX), 'vendedor3pass'), 1),
(3, 'Cliente 1', 'DNI', '44444444', 'Av. Cliente 001', '555-4444', 'cliente1@example.com', CONVERT(VARBINARY(MAX), 'cliente1pass'), 1),
(3, 'Cliente 2', 'DNI', '55555555', 'Av. Cliente 002', '555-5555', 'cliente2@example.com', CONVERT(VARBINARY(MAX), 'cliente2pass'), 1),
(4, 'Gerente', 'DNI', '66666666', 'Av. Gerente 001', '555-6666', 'gerente@example.com', CONVERT(VARBINARY(MAX), 'gerentepass'), 1),
(5, 'Soporte', 'DNI', '77777777', 'Av. Soporte 001', '555-7777', 'soporte@example.com', CONVERT(VARBINARY(MAX), 'soportepass'), 1),
(6, 'Contador', 'DNI', '88888888', 'Av. Contador 001', '555-8888', 'contador@example.com', CONVERT(VARBINARY(MAX), 'contadorpass'), 1),
(7, 'Analista', 'DNI', '99999999', 'Av. Analista 001', '555-9999', 'analista@example.com', CONVERT(VARBINARY(MAX), 'analistapass'), 1);

-- Insertar registros en la tabla ingreso
INSERT INTO ingreso (idproveedor, idusuario, tipo_comprobante, serie_comprobante, num_comprobante, fecha, impuesto, total, estado) VALUES
(2, 1, 'Factura', 'F001', '00000001', '2024-01-01 10:00:00', 18.00, 1000.00, 'Pendiente'),
(4, 2, 'Boleta', 'B001', '00000002', '2024-01-02 11:00:00', 10.00, 500.00, 'Pendiente'),
(6, 3, 'Factura', 'F001', '00000003', '2024-01-03 12:00:00', 18.00, 200.00, 'Pendiente'),
(2, 4, 'Boleta', 'B001', '00000004', '2024-01-04 13:00:00', 10.00, 150.00, 'Pendiente'),
(4, 5, 'Factura', 'F001', '00000005', '2024-01-05 14:00:00', 18.00, 600.00, 'Pendiente'),
(6, 6, 'Boleta', 'B001', '00000006', '2024-01-06 15:00:00', 10.00, 700.00, 'Pendiente'),
(2, 7, 'Factura', 'F001', '00000007', '2024-01-07 16:00:00', 18.00, 900.00, 'Pendiente'),
(4, 8, 'Boleta', 'B001', '00000008', '2024-01-08 17:00:00', 10.00, 450.00, 'Pendiente'),
(6, 9, 'Factura', 'F001', '00000009', '2024-01-09 18:00:00', 18.00, 800.00, 'Pendiente'),
(2, 10, 'Boleta', 'B001', '00000010', '2024-01-10 19:00:00', 10.00, 350.00, 'Pendiente');

-- Insertar registros en la tabla detalle_ingreso
INSERT INTO detalle_ingreso (idingreso, idarticulo, cantidad, precio) VALUES
(1, 1, 2, 450.00),
(2, 2, 1, 250.00),
(3, 3, 4, 50.00),
(4, 4, 5, 30.00),
(5, 5, 6, 150.00),
(6, 6, 7, 175.00),
(7, 7, 1, 220.00),
(8, 8, 2, 225.00),
(9, 9, 3, 200.00),
(10, 10, 4, 175.00);

-- Insertar registros en la tabla venta
INSERT INTO venta (idcliente, idusuario, tipo_comprobante, serie_comprobante, num_comprobante, fecha, impuesto, total, estado) VALUES
(1, 1, 'Factura', 'F001', '00000001', '2024-01-01 10:00:00', 18.00, 1000.00, 'Pendiente'),
(2, 2, 'Boleta', 'B001', '00000002', '2024-01-02 11:00:00', 10.00, 500.00, 'Pendiente'),
(3, 3, 'Factura', 'F001', '00000003', '2024-01-03 12:00:00', 18.00, 200.00, 'Pendiente'),
(4, 4, 'Boleta', 'B001', '00000004', '2024-01-04 13:00:00', 10.00, 150.00, 'Pendiente'),
(5, 5, 'Factura', 'F001', '00000005', '2024-01-05 14:00:00', 18.00, 600.00, 'Pendiente'),
(6, 6, 'Boleta', 'B001', '00000006', '2024-01-06 15:00:00', 10.00, 700.00, 'Pendiente'),
(7, 7, 'Factura', 'F001', '00000007', '2024-01-07 16:00:00', 18.00, 900.00, 'Pendiente'),
(8, 8, 'Boleta', 'B001', '00000008', '2024-01-08 17:00:00', 10.00, 450.00, 'Pendiente'),
(9, 9, 'Factura', 'F001', '00000009', '2024-01-09 18:00:00', 18.00, 800.00, 'Pendiente'),
(10, 10, 'Boleta', 'B001', '00000010', '2024-01-10 19:00:00', 10.00, 350.00, 'Pendiente');

-- Insertar registros en la tabla detalle_venta
INSERT INTO detalle_venta (idventa, idarticulo, cantidad, precio, descuento) VALUES
(1, 1, 2, 450.00, 20.00),
(2, 2, 1, 250.00, 10.00),
(3, 3, 4, 50.00, 5.00),
(4, 4, 5, 30.00, 2.00),
(5, 5, 6, 150.00, 15.00),
(6, 6, 7, 175.00, 20.00),
(7, 7, 1, 220.00, 10.00),
(8, 8, 2, 225.00, 8.00),
(9, 9, 3, 200.00, 12.00),
(10, 10, 4, 175.00, 5.00);
```

**Nota: En MariaDB usar UNHEX(HEX('adminpass')), y así realizarlo con todas las contraseñas**