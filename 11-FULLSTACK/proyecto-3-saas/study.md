# Proyecto 3: SaaS Básico

## Descripción

Construir una aplicación SaaS básica multi-tenant.

## Requisitos

- Sistema multi-tenant
- Autenticación de usuarios
- Dashboard básico
- Base de datos separada por tenant

## Stack

- **Frontend**: React
- **Backend**: Python/FastAPI
- **DB**: PostgreSQL

## Arquitectura Multi-Tenant

```python
from fastapi import FastAPI, Depends
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import relationship

app = FastAPI()

class Tenant(Base):
    __tablename__ = "tenants"
    
    id = Column(Integer, primary_key=True)
    subdomain = Column(String, unique=True)
    name = Column(String)
    users = relationship("User", back_populates="tenant")

class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True)
    tenant_id = Column(Integer, ForeignKey("tenants.id"))
    email = Column(String)
    password_hash = Column(String)
    
    tenant = relationship("Tenant", back_populates="users")
```

## Features

1. **Registro/Login**: JWT auth
2. **Dashboard**: Stats del tenant
3. **Users CRUD**: Gestión de usuarios
4. **Settings**: Configuración del tenant

## Entregable

1. App funcional
2. DB schema
3. Tests

## Evaluación

- Funcionalidad multi-tenant
- Seguridad
- Código limpio