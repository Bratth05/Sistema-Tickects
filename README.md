docker pull mysql:innovation-oraclelinux9
$ docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=Ceutec1* -d mysql:innovation-oraclelinux9

USE SistemaTickets;

--DDL

CREATE TABLE IF NOT EXISTS Usuario (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(100) NOT NULL,
    telefono VARCHAR(20)
);

CREATE TABLE IF NOT EXISTS Ingeniero (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(100) NOT NULL,
    especialidad VARCHAR(50),
    cargaLaboral INT DEFAULT 0
);

CREATE TABLE IF NOT EXISTS Categoria (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);

CREATE TABLE IF NOT EXISTS Ticket (
    id INT AUTO_INCREMENT PRIMARY KEY,
    usuarioId INT,
    ingenieroId INT,
    categoriaId INT,
    descripcion TEXT NOT NULL,
    fechaApertura TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    fechaCierre TIMESTAMP NULL,
    estado VARCHAR(20) DEFAULT 'Abierto',
    FOREIGN KEY (usuarioId) REFERENCES Usuario(id),
    FOREIGN KEY (ingenieroId) REFERENCES Ingeniero(id),
    FOREIGN KEY (categoriaId) REFERENCES Categoria(id)
);

CREATE TABLE IF NOT EXISTS Procedimiento (
    id INT AUTO_INCREMENT PRIMARY KEY,
    ticketId INT,
    descripcion TEXT NOT NULL,
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (ticketId) REFERENCES Ticket(id)
);

CREATE TABLE IF NOT EXISTS Calificacion (
    id INT AUTO_INCREMENT PRIMARY KEY,
    ticketId INT,
    puntuacion INT CHECK (puntuacion >= 1 AND puntuacion <= 5),
    comentario TEXT,
    FOREIGN KEY (ticketId) REFERENCES Ticket(id)
);

--DML


INSERT INTO Usuario (nombre, correo, telefono) 
VALUES ('Juan Perez', 'juan.perez@example.com', '1234567890'),
       ('Maria Gomez', 'maria.gomez@example.com', '0987654321');
       
 INSERT INTO Ingeniero (nombre, correo, especialidad, cargaLaboral) 
VALUES ('Bobadilla Ruiz', 'Bobadilla.ruiz@example.com', 'Redes', 3),
       ('Jerry Benstong', 'benstong.torres@example.com', 'Hardware', 2);

INSERT INTO Ingeniero (nombre, correo, especialidad, cargaLaboral) 
SELECT 'Bobadilla Ruiz', 'Bobadilla.ruiz@example.com', 'Redes', 3 FROM DUAL
UNION ALL
SELECT 'Jerry Benstong', 'benstong.torres@example.com', 'Hardware', 2 FROM DUAL;

