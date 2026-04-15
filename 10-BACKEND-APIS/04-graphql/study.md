# GraphQL

## Teoría

- **Schema-definition**: Tipos, queries, mutations
- **Single endpoint**: Una URL para todas las operaciones
- **Query language**: SELECT de datos específicos
- **Resolver functions**: Lógica de resolución
- **Subscriptions**: Eventos en tiempo real

## Código MuServer

```cpp
// No implementado en MuServer original
// Ejemplo de cómo sería:
/*
type Query {
    character(id: ID!): Character
    inventory(charId: ID!): [Item]
}

type Mutation {
    transferItem(from: ID!, to: ID!, item: ID!): Boolean
}
*/
```

## Ejercicio

1. Diseñar schema GraphQL para Mu Online
2. Mapear operaciones existentes a queries