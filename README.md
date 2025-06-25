# Firebase Readme

Este repositorio contiene una introducción básica a Firebase y los comandos esenciales para comenzar a trabajar con esta plataforma.

## ¿Qué es Firebase?
Firebase es una plataforma de desarrollo de aplicaciones de Google que proporciona servicios backend listos para usar, como base de datos en tiempo real, autenticación, almacenamiento, hosting, funciones en la nube y más. Es muy utilizada para aplicaciones web y móviles.

## Conexión con FlutterFlow
Para conectar tu proyecto de Firebase con FlutterFlow, es necesario agregar el siguiente correo como miembro del proyecto:

```
firebase@flutterflow.io
```

### Permisos recomendados para este correo
- **Administrador de Cloud Function**: Permite a FlutterFlow desplegar y administrar funciones en la nube.
- **Usuario de cuenta de servicio**: Permite a FlutterFlow interactuar con los recursos de Firebase usando cuentas de servicio.

Puedes agregar estos permisos desde la consola de Google Cloud, en la sección IAM (Identidad y Acceso).

#### Otros tipos de permisos útiles
- **Editor de Firestore**: Permite modificar la base de datos.
- **Administrador de autenticación**: Permite gestionar usuarios y proveedores de autenticación.
- **Administrador de almacenamiento**: Permite gestionar archivos en Firebase Storage.
- **Visualizador**: Solo permite ver la configuración y datos, sin modificar.

## Sugerencia de reglas para Firestore
Asegúrate de definir reglas de seguridad adecuadas. Un ejemplo básico para desarrollo:

```js
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true; // Solo para pruebas, no usar en producción
    }
  }
}
```

Para producción, restringe el acceso según la autenticación del usuario:

```js
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## Interactuar con la base de datos desde Firebase CLI
Puedes usar el CLI para importar, exportar y consultar datos:

- Exportar datos:
  ```sh
  firebase firestore:export ./backup
  ```
- Importar datos:
  ```sh
  firebase firestore:import ./backup
  ```
- Consultar documentos:
  ```sh
  firebase firestore:documents list /collection
  ```

Consulta la [documentación oficial](https://firebase.google.com/docs/firestore/manage-data/export-import) para más detalles.

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
