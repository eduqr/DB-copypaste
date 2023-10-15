CREATE DATABASE Negocio24BM;
GO

USE Negocio24BM;
GO

CREATE TABLE Departamento (
    PkDepartamento INT PRIMARY KEY IDENTITY(1, 1),
    Nombre VARCHAR(50)
)

CREATE TABLE Puesto (
    PkPuesto INT PRIMARY KEY IDENTITY(1, 1),
    Nombre VARCHAR(50)
)

CREATE TABLE Ciudad (
    Id INT PRIMARY KEY IDENTITY(1,1),
    Abreviatura VARCHAR(10),
    Nombre VARCHAR(50)
)

CREATE TABLE Empleado (
    PkEmpleado INT PRIMARY KEY IDENTITY(1, 1),
    Nombre VARCHAR(50),
    Apellido VARCHAR(50),
    Dirección VARCHAR(50),
    Ciudad VARCHAR(20),
    FkPuesto INT,
    FkDepartamento INT,
    FOREIGN KEY (FkPuesto) REFERENCES Puesto(PkPuesto),
    FOREIGN KEY (FkDepartamento) REFERENCES Departamento(PkDepartamento)
)