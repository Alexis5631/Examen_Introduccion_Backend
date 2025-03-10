# Examen_Introduccion_Backend

1.Modelo Conceptual

![modelo conceptual](https://github.com/user-attachments/assets/32a86641-b4db-48bf-9d53-fbcb25186f95)

2.Modelo Fisico

![modelo fisico](https://github.com/user-attachments/assets/7870ba7e-aa88-48a4-a6d0-18df89c3e1fe)

# Base de Datos: ConcesionarioDB

Este archivo describe la estructura de la base de datos `ConcesionarioDB`, utilizada para gestionar la venta de vehículos, mantenimiento y clientes de un concesionario.

## Estructura de la Base de Datos

### Creación de la Base de Datos
```sql
CREATE DATABASE ConcesionarioDB;
USE ConcesionarioDB;
```

### Tablas y Relaciones

#### Tabla INVENTARIO
```sql
CREATE TABLE INVENTARIO (
    idInventario INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT
);
```

#### Tabla CONCESIONARIO
```sql
CREATE TABLE CONCESIONARIO (
    idConcesionario INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    direccion VARCHAR(255),
    telefono VARCHAR(20)
);
```

#### Tabla VEHICULOS
```sql
CREATE TABLE VEHICULOS (
    idVehiculo INT PRIMARY KEY AUTO_INCREMENT,
    marca VARCHAR(50) NOT NULL,
    modelo VARCHAR(50) NOT NULL,
    anio INT NOT NULL,
    color VARCHAR(30),
    precio DECIMAL(10,2) NOT NULL,
    idInventario INT,
    idConcesionario INT,
    FOREIGN KEY (idInventario) REFERENCES INVENTARIO(idInventario),
    FOREIGN KEY (idConcesionario) REFERENCES CONCESIONARIO(idConcesionario)
);
```

#### Tabla VENDEDORES
```sql
CREATE TABLE VENDEDORES (
    idVendedor INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    telefono VARCHAR(20),
    email VARCHAR(100) UNIQUE
);
```

#### Tabla CLIENTES
```sql
CREATE TABLE CLIENTES (
    idCliente INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    telefono VARCHAR(20),
    email VARCHAR(100) UNIQUE
);
```

#### Tabla VENTAS
```sql
CREATE TABLE VENTAS (
    idVenta INT PRIMARY KEY AUTO_INCREMENT,
    fecha DATE NOT NULL,
    total DECIMAL(10,2) NOT NULL,
    idVendedor INT,
    idCliente INT,
    idTipoPago INT,
    FOREIGN KEY (idVendedor) REFERENCES VENDEDORES(idVendedor),
    FOREIGN KEY (idCliente) REFERENCES CLIENTES(idCliente),
    FOREIGN KEY (idTipoPago) REFERENCES TIPO_PAGO(idTipoPago)
);
```

#### Tabla DETALLE_VENTA
```sql
CREATE TABLE DETALLE_VENTA (
    idDetalle INT PRIMARY KEY AUTO_INCREMENT,
    idVenta INT,
    idVehiculo INT,
    precioVenta DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (idVenta) REFERENCES VENTAS(idVenta),
    FOREIGN KEY (idVehiculo) REFERENCES VEHICULOS(idVehiculo)
);
```

#### Tabla ORDEN_MANTENIMIENTO
```sql
CREATE TABLE ORDEN_MANTENIMIENTO (
    idOrden INT PRIMARY KEY AUTO_INCREMENT,
    fecha DATE NOT NULL,
    descripcion TEXT,
    idCliente INT,
    idVehiculo INT,
    FOREIGN KEY (idCliente) REFERENCES CLIENTES(idCliente),
    FOREIGN KEY (idVehiculo) REFERENCES VEHICULOS(idVehiculo)
);
```

#### Tabla MANTENIMIENTO
```sql
CREATE TABLE MANTENIMIENTO (
    idMantenimiento INT PRIMARY KEY AUTO_INCREMENT,
    tipo VARCHAR(100) NOT NULL,
    costo DECIMAL(10,2) NOT NULL,
    idOrden INT,
    FOREIGN KEY (idOrden) REFERENCES ORDEN_MANTENIMIENTO(idOrden)
);
```

#### Tabla TIPO_PAGO
```sql
CREATE TABLE TIPO_PAGO (
    idTipoPago INT PRIMARY KEY AUTO_INCREMENT,
    metodo VARCHAR(50) NOT NULL
);
```

## Relaciones
- Un **Inventario** puede tener múltiples **Vehículos**.
- Un **Concesionario** puede tener múltiples **Vehículos**.
- Un **Vehículo** puede estar en múltiples **Ventas**.
- Un **Vendedor** realiza múltiples **Ventas**.
- Un **Cliente** puede realizar múltiples **Órdenes de Mantenimiento**.
- Una **Orden de Mantenimiento** puede incluir múltiples **Mantenimientos**.
- Una **Venta** está asociada a un **Tipo de Pago**.

Este esquema permite gestionar la venta y mantenimiento de vehículos dentro del concesionario de manera eficiente.

