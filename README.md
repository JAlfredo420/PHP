# 🚀 Proyectos PHP e Integración IoT

Esta documentación agrupa varios proyectos desarrollados con PHP, MySQL y tecnologías asociadas para el registro y visualización de datos.

---

## 📊 Proyecto: Registro en Tabla

Este proyecto se enfoca en crear una tabla en una base de datos MySQL para almacenar datos capturados por sensores y presentarlos en una página web. Se utilizan PHP y MySQL para gestionar tanto la base de datos como la visualización de la información.

### 📖 Tecnologías Utilizadas
- *PHP*: Para establecer la conexión y manipular la base de datos.
- *MySQL*: Para el almacenamiento de la información.
- *HTML*: Para mostrar la tabla en el sitio web.
- *Servidor Web*: Apache u otro compatible con PHP y MySQL.

### 🛠 Configuración de la Base de Datos

Antes de iniciar el proyecto, verifica que la base de datos esté correctamente configurada.

#### 1️⃣ Paso 1: Crear la Base de Datos y la Tabla

Ejecuta el siguiente script SQL en phpMyAdmin o mediante la consola de MySQL
```sql
CREATE DATABASE base de datos;

USE base de datos;

CREATE TABLE registro (
    id INT AUTO_INCREMENT PRIMARY KEY,
    sensor VARCHAR(50) NOT NULL,
    fecha_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    valor FLOAT NOT NULL
);
```
### Archivos del Proyecto
### 1️⃣ db_config.php - Configuración de la Conexión a la Base de Datos
Este archivo almacena la configuración de la conexión a MySQL.
```
<?php
$servername = "alfredohr.shop";
$username = "jose_shagy";
$password = "Shagy123";
$dbname = "jose_romerillo";

// Crear conexión
$conn = new mysqli($servername, $username, $password, $dbname);

// Verificar conexión
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
?>
```
## 2️⃣ insert.php - Insertar Datos en la Tabla
Este script permite insertar datos en la tabla registro desde PHP.
```
<?php
include 'db_config.php';

$sql = "INSERT INTO registro (sensor, valor) VALUES ('temperatura', 22)";

if ($conn->query($sql) === TRUE) {
    echo "Nuevo registro creado exitosamente";
} else {
    echo "Error: " . $sql . "<br>" . $conn->error;
}

$conn->close();
?>
```
## 3️⃣ index.php - Mostrar Datos en una Tabla
Este script obtiene y muestra los datos almacenados en la base de datos.
```
<?php
include 'db_config.php';

$sql = "SELECT id, sensor, fecha_hora, valor FROM registro";
$result = $conn->query($sql);

echo "<h2>Registro de Sensores</h2>";
echo "<table border='1'>
        <tr>
        <th>ID</th>
        <th>Sensor</th>
        <th>Fecha y Hora</th>
        <th>Valor</th>
        </tr>";

if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        echo "<tr>
                <td>" . $row["id"] . "</td>
                <td>" . $row["sensor"] . "</td>
                <td>" . $row["fecha_hora"] . "</td>
                <td>" . $row["valor"] . "</td>
            </tr>";
    }
    echo "</table>";
} else {
    echo "No hay registros.";
}

$conn->close();
?>
```
## 📌 Proyecto Gestión de Sensores
Este proyecto consiste en una API desarrollada en PHP que recibe información en formato JSON y la guarda en una base de datos MySQL. Es perfecto para su integración con sistemas IoT o cualquier aplicación que necesite almacenar datos de sensores en tiempo real.
#### 🛠 Configuración de la Base de Datos
Antes de poner en marcha el proyecto, asegúrate de tener la base de datos correctamente configurada.

## 1️⃣ Crear la Base de Datos y la Tabla
Ejecuta el siguiente comando SQL en phpMyAdmin o en la consola de MySQL:
```
CREATE DATABASE admin_romerillo;  

USE admin_romerillo;  

CREATE TABLE registro (  
    id INT AUTO_INCREMENT PRIMARY KEY,  
    sensor VARCHAR(50) NOT NULL,  
    fecha_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
    valor FLOAT NOT NULL  
);  
```
## 🚀 Archivos del Proyecto
## 1️⃣ db_config.php - Configuración de la Conexión con la Base de Datos
Este archivo se encarga de establecer la conexión entre PHP y MySQL.
```
<?php
$servername = "pagina web";
$username = "nombre de acceso";
$password = "contraseña";
$dbname = "nombre de la base de datos";

// Crear conexión
$conn = new mysqli($servername, $username, $password, $dbname);

// Verificar conexión
if ($conn->connect_error) {
    die("Conexión fallida: " . $conn->connect_error);
}
?>
```
## 2️⃣ recibir_datos.php - Procesar y Almacenar los Datos
Este archivo se encarga de recibir los datos en formato JSON mediante una solicitud POST, procesarlos y almacenarlos en la base de datos.
```
<?php
include 'db_config.php';

// Verificar si la solicitud es POST
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    // Obtener los datos JSON recibidos
    $datos = json_decode(file_get_contents('php://input'));

    if (isset($datos->sensor) && isset($datos->valor)) {
        $sensor = $conn->real_escape_string($datos->sensor);
        $valor = $conn->real_escape_string($datos->valor);

        // Insertar datos en la base de datos
        $sql = "INSERT INTO registro (sensor, valor) VALUES ('$sensor', '$valor')";

        if ($conn->query($sql) === TRUE) {
            echo json_encode(["mensaje" => "Nuevo registro creado exitosamente"]);
        } else {
            echo json_encode(["error" => "Error al insertar: " . $conn->error]);
        }
    } else {
        echo json_encode(["error" => "Datos incompletos"]);
    }
}

// Cerrar conexión
$conn->close();
?>
```
## 📌 Proyecto Dientes (Bluetooth)
Este proyecto implementa un sistema para transmitir datos desde un ESP32 a un servidor PHP utilizando Bluetooth Serial. Una vez que los datos se reciben a través de Bluetooth, se envían a una base de datos MySQL a través de una API en PHP mediante conexión WiFi.

## 📖 Tecnologías Utilizadas

- ESP32 (Placa de desarrollo)

- Bluetooth Serial (Para recibir datos desde un dispositivo móvil u otro ESP32)

- WiFi (Para transmitir datos al servidor web)

- PHP (Para recibir y guardar los datos en una base de datos MySQL)

- MySQL (Para administrar la base de datos)

- Arduino IDE (Para programar el ESP32)

## 📁 Estructura del Proyecto

- 📄 db_config.php # Configuración de la base de datos

- 📄 recibir_datos.php # API que recibe los datos y los almacena en la BD

- 📄 ESP32_Bluetooth.ino # Código del ESP32 para recibir datos y enviarlos

## 🚀 Archivos del Proyecto
#### 📌 db_config.php - Configuración de la Conexión con la Base de Datos
Este archivo establece la conexión entre PHP y MySQL.

```
<?php
$servername = "pagina web";
$username = "nombre de usuario";
$password = "contraseña";
$dbname = "nombre de la base de datos";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("Conexión fallida: " . $conn->connect_error);
}
?>
```
### 📌 recibir_datos.php - API para Procesar y Almacenar Datos
Este archivo recibe los datos en formato JSON mediante una solicitud POST y los guarda en la base de datos.
```
<?php
include 'db_config.php';

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $datos = json_decode(file_get_contents('php://input'));

    if (isset($datos->sensor) && isset($datos->valor)) {
        $sensor = $conn->real_escape_string($datos->sensor);
        $valor = $conn->real_escape_string($datos->valor);

        $sql = "INSERT INTO registro (sensor, valor) VALUES ('$sensor', '$valor')";

        if ($conn->query($sql) === TRUE) {
            echo json_encode(["mensaje" => "Datos registrados correctamente"]);
        } else {
            echo json_encode(["error" => "Error al insertar: " . $conn->error]);
        }
    } else {
        echo json_encode(["error" => "Datos incompletos"]);
    }
}

$conn->close();
?>
```
