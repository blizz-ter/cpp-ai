# Proyecto 5: Plataforma de Streaming

## Descripción

Crear una plataforma de streaming tipo Netflix/YouTube.

## Requisitos

- Upload de videos
- Streaming adaptativo
- Comentarios y likes
- Suscripciones

## Stack

- **Frontend**: React + Video.js
- **Backend**: Node.js + FFmpeg
- **Storage**: AWS S3 / MinIO
- **DB**: PostgreSQL

## Arquitectura

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│  API Server │────▶│  Database   │
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │   Storage   │
                    │  (S3/MinIO) │
                    └─────────────┘
```

## Modelos

### Video

```javascript
const videoSchema = new mongoose.Schema({
    title: String,
    description: String,
    url: String,
    thumbnail: String,
    duration: Number,
    views: { type: Number, default: 0 },
    likes: { type: Number, default: 0 },
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    createdAt: { type: Date, default: Date.now }
});
```

### Comment

```javascript
const commentSchema = new mongoose.Schema({
    video: { type: mongoose.Schema.Types.ObjectId, ref: 'Video' },
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    content: String,
    createdAt: { type: Date, default: Date.now }
});
```

## Features

1. **Video Player**: Video.js con HLS
2. **Upload**: Upload con progress
3. **Comments**: Sistema de comentarios
4. **Likes**: Like/unlike videos
5. **Search**: Búsqueda de videos
6. **Recommendations**: Videos relacionados

## Entregable

1. Plataforma funcional
2. Sistema de streaming
3. Tests

## Evaluación

- Streaming fluida
- Funcionalidad completa
- Código mantenible