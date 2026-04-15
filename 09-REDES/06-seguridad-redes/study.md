# Seguridad en Redes

## Teoría

- **Cifrado**: TLS/SSL, AES, RSA
- **Autenticación**: Certificados digitales, PKI
- **Firewall**: Filtrado de paquetes, IDS/IPS
- **VPN**: Tuneles cifrados
- **Ataques comunes**: MITM, DoS, inyección de paquetes

## Código MuServer

```cpp
// encryption.cpp (verificar existencia)
//Funciones de cifrado de packets
extern "C" {
    void EncryptData(char* buffer, int size);
    void DecryptData(char* buffer, int size);
}
```

## Ejercicio

1. Buscar funciones de cifrado en Source MuServer
2. Documentar como se secure los paquetes