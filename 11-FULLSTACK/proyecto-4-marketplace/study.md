# Proyecto 4: Marketplace

## Descripción

Crear un marketplace con vendedores independientes.

## Requisitos

- Registro de vendedores
- Catálogo de productos
- Carrito de compras
- Sistema de pagos (mock)
- Órdenes

## Stack

- **Frontend**: React + Redux
- **Backend**: Node.js/Express
- **DB**: MongoDB + Redis

## Modelos de Datos

### Producto

```javascript
const productSchema = new mongoose.Schema({
    seller: { type: mongoose.Schema.Types.ObjectId, ref: 'Seller' },
    name: String,
    description: String,
    price: Number,
    stock: Number,
    images: [String],
    category: String,
    createdAt: { type: Date, default: Date.now }
});
```

### Orden

```javascript
const orderSchema = new mongoose.Schema({
    buyer: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    products: [{
        product: { type: mongoose.Schema.Types.ObjectId, ref: 'Product' },
        quantity: Number,
        price: Number
    }],
    total: Number,
    status: { type: String, enum: ['pending', 'paid', 'shipped', 'delivered'] },
    createdAt: { type: Date, default: Date.now }
});
```

## API Endpoints

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | /api/products | Listar productos |
| GET | /api/products/:id | Ver producto |
| POST | /api/products | Crear producto (vendedor) |
| POST | /api/cart | Agregar al carrito |
| POST | /api/orders | Crear orden |

## Features

1. **Browse**: Ver productos por categoría
2. **Search**: Buscar productos
3. **Cart**: Carrito de compras
4. **Checkout**: Proceso de compra
5. **Orders**: Seguimiento de órdenes

## Entregable

1. Marketplace funcional
2. Tests unitarios
3. Documentación API

## Evaluación

- Completitud de features
- UX/UI
- Código