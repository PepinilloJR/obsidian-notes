
La autenticacion es el proceso de identificar a un usuario y garantizar que sea quien dice ser

La autorización es lo que define que recursos del sistema accede el usuario anteriormente autenticado

JWT o JSON Web Token es un estándar de token de seguridad que se utiliza para autenticar y autorizar usuarios en aplicaciones web y móviles

La autorización JWT sera el proceso de identificar un usuario y conceder acceso a los recursos específicos de este.

Un JWT es esencialmente un objeto JSON codificado que contiene informacion sobre el usuario, firmado digitalmente para garantizar que es autentico e integro

### Paso a paso

El usuario inicia sesión en la aplicación, el servidor genera un JWT lo devuelve al cliente, luego el cliente incluye este JWT en todas las solicitudes posteriores para demostrar que son peticiones validas.

## Implementación JWT usando jsonwebtoken

```
npm install jsonwebtoken
```

