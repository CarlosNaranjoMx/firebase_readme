# Firebase Readme

Este repositorio contiene una introducción básica a Firebase y los comandos esenciales para comenzar a trabajar con esta plataforma.

## ¿Qué es Firebase?
Firebase es una plataforma de desarrollo de aplicaciones de Google que proporciona servicios backend listos para usar, como base de datos en tiempo real, autenticación, almacenamiento, hosting, funciones en la nube y más. Es muy utilizada para aplicaciones web y móviles.

## Comandos básicos de Firebase CLI
Para trabajar con Firebase desde la terminal, necesitas instalar el [Firebase CLI](https://firebase.google.com/docs/cli):

```sh
npm install -g firebase-tools
```

### Inicializar un proyecto
```sh
firebase init
```
Permite configurar los servicios de Firebase que usarás (Firestore, Functions, Hosting, etc.)

### Autenticarse en Firebase
```sh
firebase login
```
Inicia sesión con tu cuenta de Google.

### Desplegar servicios
```sh
firebase deploy
```
Sube tu configuración, funciones, reglas y hosting a Firebase.

### Emuladores locales
```sh
firebase emulators:start
```
Inicia los emuladores locales de los servicios configurados.

### Ver estado del proyecto
```sh
firebase projects:list
```
Muestra los proyectos de Firebase asociados a tu cuenta.

### Otras operaciones útiles
- `firebase logout` — Cierra la sesión actual.
- `firebase use --add` — Cambia o agrega un proyecto activo en tu carpeta local.
- `firebase functions:log` — Muestra los logs de Cloud Functions.

## Recursos
- [Documentación oficial de Firebase](https://firebase.google.com/docs)
- [Referencia de comandos CLI](https://firebase.google.com/docs/cli#commands)

---

Para dudas o sugerencias, contacta a CarlosNaranjoMx.
