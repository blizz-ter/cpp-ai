# Testing de Servidores

## Teoría

- **Unit tests**: Pruebas de funciones individuales
- **Integration tests**: Pruebas de componentes
- **Load testing**: Pruebas de carga (JMeter, k6)
- **Stress testing**: Prueba de límites
- **Mock servers**: Simulación de servicios

## Código MuServer

```cpp
//测试 no tradicional en MuServer
// Unit test manual:
void TestAttack() {
    // Setup
    gObjMock = CreateMockPlayer();
    // Execute
    gAttack.CGAttackRecv(mock, packet);
    // Assert
    ASSERT_EQ(mock.damage, expected);
}
```

## Ejercicio

1. Crear tests unitarios para funciones simples
2. Implementar load test con múltiples clientes