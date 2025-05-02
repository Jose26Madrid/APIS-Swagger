
# FastAPI con Swagger personalizado en EC2

Este proyecto muestra cómo crear una API REST usando **FastAPI** desplegada en una instancia EC2 de Amazon Linux, incluyendo una documentación Swagger personalizada.

---

## 🚀 Requisitos

- Una instancia EC2 corriendo Amazon Linux 2023
- Python 3 ya instalado (`python3 --version`)
- Acceso a internet y permisos de seguridad para abrir puertos en el Security Group

---

## 🔧 Instalación de pip

```bash
sudo yum install python3-pip
# Si no funciona, usa:
curl -O https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

---

## 📦 Instalación de FastAPI y Uvicorn

```bash
pip3 install fastapi uvicorn
```

---

## 📝 Código del archivo `main2.py`

```python
from fastapi import FastAPI

app = FastAPI(
    title="Mi API personalizada",
    description="Documentación personalizada de Swagger UI",
    version="1.0.0",
    docs_url="/documentacion",
    redoc_url="/redoccion",
    openapi_url="/api/openapi.json"
)

@app.get("/saludo", summary="Saludar al usuario con estilo")
def saludar(nombre: str = "Mundo", idioma: str = "es"):
    if idioma == "en":
        mensaje = f"Hello, {nombre}!"
    elif idioma == "fr":
        mensaje = f"Bonjour, {nombre}!"
    elif idioma == "it":
        mensaje = f"Ciao, {nombre}!"
    else:
        mensaje = f"Hola, {nombre}!"
    return {"mensaje": mensaje}
```

---

## ▶️ Ejecutar Uvicorn

```bash
uvicorn main2:app --host 0.0.0.0 --port 8000 --reload
```

---

## 🌐 Acceso desde el navegador o curl

```bash
curl "http://<IP_PUBLICA_EC2>:8000/saludo?nombre=Ana&idioma=fr"
```

### Documentación:

- Swagger UI: `http://<IP_PUBLICA_EC2>:8000/documentacion`
- ReDoc: `http://<IP_PUBLICA_EC2>:8000/redoccion`
- OpenAPI JSON: `http://<IP_PUBLICA_EC2>:8000/api/openapi.json`

---

## 🔒 Seguridad: abrir puerto 8000 en EC2

1. Ir a la consola EC2 de AWS.
2. Editar el Security Group de la instancia.
3. Añadir una **regla de entrada**:
   - Tipo: Custom TCP
   - Puerto: 8000
   - Fuente: `0.0.0.0/0` (o tu IP para más seguridad)

---

## ✅ Estado

Funcional con acceso externo confirmado usando `curl`.

### MIT License
### Copyright (c) 2025 Jose Magariño
### See LICENSE file for more details.
