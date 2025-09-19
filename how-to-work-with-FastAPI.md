# 🚀 How to Work with FastAPI – A Complete Beginner’s Guide

![FastAPI Logo](https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png)

FastAPI has quickly become one of the most popular frameworks for building modern web APIs in Python. It is **fast**, **easy to use**, and **developer-friendly**. In this blog, we’ll explore how to work with FastAPI, from installation to building and running your first API.  

By the end, you’ll be able to build your own production-ready APIs with FastAPI! 🎉  

---

## 📌 What is FastAPI?

[FastAPI](https://fastapi.tiangolo.com/) is a **high-performance Python web framework** for building APIs. It is built on top of **Starlette** (for the web parts) and **Pydantic** (for data validation).  

### ✅ Key Features
- **Lightning Fast** – Performance on par with Node.js and Go.  
- **Automatic Docs** – Swagger UI & ReDoc are generated automatically.  
- **Data Validation** – Built-in with Pydantic.  
- **Async Ready** – Fully supports async & await for non-blocking requests.  
- **Easy to Use** – Simple Python code to define APIs.  

---

## ⚡ Installation

To get started, you just need Python 3.7+ installed. Let’s create a virtual environment and install FastAPI:

```bash

# Create and activate a virtual environment (Linux / Mac)
python3 -m venv venv
source venv/bin/activate

# For Windows
python -m venv venv
venv\Scripts\activate

# Install FastAPI and Uvicorn (ASGI server)
pip install fastapi uvicorn

```
### 🛠️ Building Your First API

Let’s create a simple API with FastAPI that returns a “Hello World” message.

### 📄 File: main.py
```python
from fastapi import FastAPI

# Create FastAPI app instance
app = FastAPI()

# Define a root endpoint
@app.get("/")
def read_root():
    return {"message": "Hello World from FastAPI 🚀"}

```

### ▶️ Running the Server

Use Uvicorn, a blazing-fast ASGI server, to run the app.
```bash
uvicorn main:app --reload
```


- main:app → main is the filename (main.py), and app is the FastAPI instance.

- --reload → Enables hot reloading (restart on file changes).

Now open http://127.0.0.1:8000
 and you’ll see:
```json
{
  "message": "Hello World from FastAPI 🚀"
}
```

### 📑 Auto-generated Documentation

One of the best features of FastAPI is automatic interactive documentation.

- Swagger UI → http://127.0.0.1:8000/docs
- ReDoc → http://127.0.0.1:8000/redoc



### 📬 Path & Query Parameters

Let’s add some endpoints that accept parameters.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str | None = None):
    return {
        "item_id": item_id,
        "query": q
    }
```
### Example Requests

- http://127.0.0.1:8000/items/5

- http://127.0.0.1:8000/items/5?q=fastapi

#### Response:
```json
{
  "item_id": 5,
  "query": "fastapi"
}
```
### 📝 Request Body with Pydantic

FastAPI uses Pydantic models for data validation.
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Define a Pydantic model
class Item(BaseModel):
    name: str
    price: float
    is_offer: bool = False

@app.post("/items/")
def create_item(item: Item):
    return {"message": f"Item '{item.name}' created!", "data": item}
```
### Example Request:
```bash
curl -X POST "http://127.0.0.1:8000/items/" \
-H "Content-Type: application/json" \
-d '{"name": "Laptop", "price": 1200, "is_offer": true}'
```

Response:
```json
{
  "message": "Item 'Laptop' created!",
  "data": {
    "name": "Laptop",
    "price": 1200.0,
    "is_offer": true
  }
}
```

### ⚡ Async Endpoints

FastAPI supports async routes for non-blocking I/O.
```python
import asyncio
from fastapi import FastAPI

app = FastAPI()

@app.get("/wait")
async def wait_example():
    await asyncio.sleep(2)  # Simulate delay
    return {"status": "Done after 2 seconds ⏳"}
```
### 🛡️ Adding Middleware

Middleware lets you run code before & after every request.
```python
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def add_custom_header(request: Request, call_next):
    response = await call_next(request)
    response.headers["X-App-Name"] = "My FastAPI App"
    return response
```

Now, all responses will have a custom header:
```bash
X-App-Name: My FastAPI App
```
### 🐳 Deploying FastAPI with Docker

A minimal Dockerfile:

FROM python:3.10-slim
```dockerfile
WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Build & run:
```bash
docker build -t fastapi-app .
docker run -p 8000:8000 fastapi-app
```
### 🎯 Conclusion

FastAPI makes it incredibly easy to build modern APIs with:

- Auto docs 📑
- Built-in validation ✅
- Async support ⚡
- Production-ready deployment 🐳

If you’re starting a new API project in Python, *FastAPI should be your go-to choice.*

👉 Next Steps:

- Try connecting FastAPI with a database (PostgreSQL, MongoDB, etc.).
- Add authentication (OAuth2, JWT).
- Deploy on cloud (AWS, GCP, or Heroku).

##### ✍️ Written by Your Name
##### 📅 Published on September 2025
