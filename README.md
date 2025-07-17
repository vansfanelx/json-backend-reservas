# API de Reservas de Mesas (JSON Server)

Este proyecto proporciona una API REST simulada para un sistema de reservas de mesas de restaurante utilizando `json-server`.

## 🚀 Instalación

1. Instala json-server globalmente (si aún no lo tienes):

```bash
npm install -g json-server
```

2. Ejecuta el backend en el puerto 3001:

```bash
json-server --watch db.json --port 3001
```

---

## 📌 Endpoints disponibles

### 🔹 RESERVAS

| Método | Endpoint         | Descripción                                      |
|--------|------------------|--------------------------------------------------|
| GET    | /reservas        | Lista todas las reservas                         |
| GET    | /reservas/:id    | Obtiene una reserva por ID                       |
| POST   | /reservas        | Crea una nueva reserva                           |
| PUT    | /reservas/:id    | Reemplaza completamente una reserva              |
| DELETE | /reservas/:id    | Elimina una reserva                              |

### 🔹 MESAS

| Método | Endpoint       | Descripción                                    |
|--------|----------------|------------------------------------------------|
| GET    | /mesas         | Lista todas las mesas                          |
| GET    | /mesas/:id     | Obtiene los detalles de una mesa por ID        |
| POST   | /mesas         | Crea una nueva mesa                            |
| PUT    | /mesas/:id     | Reemplaza completamente una mesa               |
| DELETE | /mesas/:id     | Elimina una mesa                               |

---

## 👤 Endpoints del recurso `/usuarios`

Este recurso simula una tabla de usuarios para propósitos de login o filtrado por roles.

### 📄 Estructura de un usuario

```json
{
  "id": 1,
  "usuario": "admin1",
  "password": "admin123",
  "correo": "admin1@demo.com",
  "telefono": "999111222",
  "rol": "administrador"
}
```

### 🔹 Endpoints disponibles

| Método | Endpoint        | Descripción                          |
|--------|-----------------|--------------------------------------|
| GET    | /usuarios       | Lista todos los usuarios             |
| GET    | /usuarios/:id   | Obtiene un usuario por ID            |
| POST   | /usuarios       | Crea un nuevo usuario                |
| PUT    | /usuarios/:id   | Reemplaza completamente un usuario   |
| DELETE | /usuarios/:id   | Elimina un usuario                   |

---

### 🔍 Filtros útiles

```http
GET /usuarios?rol=cliente
```
Devuelve solo los usuarios con rol cliente.

```http
GET /usuarios?usuario=cliente1&password=cliente123
```
Simula un login comparando credenciales desde el frontend.

---

## 🔍 Filtros y búsqueda avanzada

`json-server` permite aplicar filtros muy útiles para trabajar con consultas dinámicas desde React.

### 🔹 Buscar por campo específico (filtro exacto)

```http
GET /reservas?cliente=Juan Pérez
```

```http
GET /reservas?estado=confirmada
```

---

### 🔹 Búsqueda de texto general (full-text)

```http
GET /reservas?q=Juan
```

---

### 🔹 Ordenar resultados

```http
GET /reservas?_sort=fecha&_order=asc
```

```http
GET /reservas?_sort=hora&_order=desc
```

---

### 🔹 Combinar filtros y orden

```http
GET /reservas?estado=pendiente&_sort=fecha&_order=desc
```

```http
GET /reservas?cliente=Ana Torres&estado=confirmada
```

---

## 🧪 Ejemplos de consumo

### 🔹 Con fetch (obtener reservas)

```js
useEffect(() => {
  fetch('http://localhost:3001/reservas')
    .then(res => res.json())
    .then(data => setReservas(data));
}, []);
```

### 🔹 Con axios (obtener reservas)

```js
import axios from 'axios';

useEffect(() => {
  axios.get('http://localhost:3001/reservas')
       .then(response => setReservas(response.data));
}, []);
```

### 🔹 POST con fetch (crear reserva)

```js
fetch('http://localhost:3001/reservas', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    cliente: "Ana Torres",
    fecha: "2025-07-22",
    hora: "19:00",
    mesa: 2,
    personas: 2,
    estado: "pendiente",
    notas: "Mesa con vista"
  })
});
```

### 🔹 POST con axios (crear mesa)

```js
axios.post('http://localhost:3001/mesas', {
  nombre: "Mesa 10",
  capacidad: 8
});
```

---

## ⚠️ Nota importante

Los datos se almacenan en memoria durante la ejecución del servidor. Si detienes el servidor (`Ctrl+C`), los datos se restablecen según `db.json`.

---

## 🧩 Demo React (useState + useEffect)

```jsx
import React, { useState, useEffect } from 'react';

function App() {
  const [reservas, setReservas] = useState([]);

  useEffect(() => {
    fetch('http://localhost:3001/reservas')
      .then(res => res.json())
      .then(data => setReservas(data));
  }, []);

  return (
    <div>
      <h1>Lista de Reservas</h1>
      <ul>
        {reservas.map(r => (
          <li key={r.id}>
            {r.fecha} - {r.hora} - {r.cliente} ({r.estado})
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```
