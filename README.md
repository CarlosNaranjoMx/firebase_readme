# Firebase Readme

Este repositorio contiene una introducción básica a Firebase y los comandos esenciales para comenzar a trabajar con esta plataforma.

## Índice
1. [Comandos básicos de Firebase CLI](#comandos-básicos-de-firebase-cli)
2. [Comandos de validación y consulta](#comandos-de-validación-y-consulta)
3. [Comandos para ver y crear schemas en Firestore](#comandos-para-ver-y-crear-schemas-en-firestore)
4. [Modificar el contenido de un esquema (documento) desde Firebase CLI](#modificar-el-contenido-de-un-esquema-documento-desde-firebase-cli)
5. [¿Qué es Firebase?](#qué-es-firebase)
6. [Conexión con FlutterFlow](#conexión-con-flutterflow)
7. [Sugerencia de reglas para Firestore](#sugerencia-de-reglas-para-firestore)
8. [Interactuar con la base de datos desde Firebase CLI](#interactuar-con-la-base-de-datos-desde-firebase-cli)
9. [Recursos](#recursos)

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

## Comandos de validación y consulta

- **Ver el correo conectado actualmente:**
  ```sh
  firebase login:list
  ```
  Muestra el correo electrónico actualmente autenticado en Firebase CLI.

- **Ver proyectos de Firebase asociados:**
  ```sh
  firebase projects:list
  ```
  Lista los proyectos de Firebase disponibles para tu cuenta.

- **Ver el proyecto activo en la carpeta actual:**
  ```sh
  firebase use
  ```
  Muestra el proyecto de Firebase actualmente seleccionado en tu carpeta local.

- **Ver configuración de la CLI:**
  ```sh
  firebase --help
  ```
  Muestra la ayuda y todos los comandos disponibles en Firebase CLI.

- **Ver detalles de configuración local:**
  ```sh
  cat .firebaserc
  ```
  Muestra el archivo de configuración local del proyecto (si existe).

## Comandos para ver y crear schemas en Firestore

Actualmente, el Firebase CLI no permite crear schemas (estructuras de colecciones y campos) de forma declarativa o mediante comandos directos. Sin embargo, puedes ver la estructura de tus datos y exportar/importar datos, lo que puede ayudarte a documentar o replicar schemas.

### Ver la estructura de colecciones y documentos

Puedes listar documentos de una colección:
```sh
firebase firestore:documents list /<coleccion>
```
Esto muestra los documentos y sus campos, permitiendo inspeccionar la estructura actual.

### Exportar la estructura y datos
```sh
firebase firestore:export ./backup
```
Esto exporta todos los datos y, al importar en otro proyecto, replica la estructura (schemas) existente.

### Crear datos (y por lo tanto schemas) desde CLI
No existe un comando específico para crear schemas, pero puedes crear documentos y colecciones usando:
```sh
firebase firestore:documents create /<coleccion>/<documentoID> --data '{"campo1": "valor", "campo2": 123}'
```
Esto crea un documento con los campos especificados, generando la estructura (schema) en Firestore.

> **Nota:** Firestore es una base de datos NoSQL y no requiere definir schemas de antemano. El schema se genera dinámicamente al crear documentos y campos.

## Modificar el contenido de un esquema (documento) desde Firebase CLI

En Firestore, los “esquemas” se generan dinámicamente al crear o modificar documentos y colecciones. Si tu app fue creada en FlutterFlow y ya tienes colecciones/documentos, puedes modificar su contenido desde la Firebase CLI usando los siguientes comandos:

### Actualizar campos de un documento existente
```sh
firebase firestore:documents update /<coleccion>/<documentoID> --data '{"campo1": "nuevo valor", "campo2": 123}'
```
Esto agrega o modifica los campos especificados en el documento.

### Agregar un nuevo campo a un documento
```sh
firebase firestore:documents update /<coleccion>/<documentoID> --data '{"nuevoCampo": "valor"}'
```

### Eliminar un campo de un documento
```sh
firebase firestore:documents update /<coleccion>/<documentoID> --deleteFields campoAEliminar
```

### Crear un documento con un nuevo “esquema”
```sh
firebase firestore:documents create /<coleccion>/<nuevoDocumentoID> --data '{"campoA": "valor", "campoB": 42}'
```
Esto crea un documento con los campos especificados, generando la estructura (schema) en Firestore.

> **Nota:** No puedes modificar la estructura de la colección como tal, solo los documentos y sus campos. Firestore es flexible y no requiere definir el esquema antes de tiempo.

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

## Ejemplo y explicación de reglas de Firebase Storage

```js
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if false;
    }
    match /users/{userId}/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth.uid == userId;
    }
  }
}
```

**¿Qué significan estas reglas?**

- `rules_version = '2';`: Indica la versión de las reglas de seguridad de Firebase Storage.
- `service firebase.storage { ... }`: Define las reglas para el servicio de almacenamiento.
- `match /b/{bucket}/o { ... }`: Aplica las reglas a todos los buckets y objetos del storage.
- `match /{allPaths=**} { allow read, write: if false; }`: Por defecto, nadie puede leer ni escribir en ningún archivo.
- `match /users/{userId}/{allPaths=**} { ... }`: Excepción para la carpeta `users`:
  - `allow read: if true;`: Cualquier usuario puede leer archivos en cualquier subcarpeta de `users`.
  - `allow write: if request.auth.uid == userId;`: Solo el usuario autenticado cuyo UID coincide con `userId` puede escribir (subir/modificar) archivos en su propia carpeta.

**Resumen:**
- Nadie puede acceder a ningún archivo salvo a los que están bajo `/users/{userId}/`.
- Todos pueden leer archivos en `/users/{userId}/`.
- Solo el usuario autenticado puede escribir en su propia carpeta.

> Ajusta estas reglas según la privacidad y seguridad que requiera tu aplicación.

---

Para dudas o sugerencias, contacta a CarlosNaranjoMx.
