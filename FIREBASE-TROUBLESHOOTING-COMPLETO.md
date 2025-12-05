# 🔧 TROUBLESHOOTING: Firebase No Disponible

## ⚠️ Error: "Firebase no disponible"

Si ves este error es porque **Firebase no está cargando correctamente**.

---

## 🔍 PASO 1: Verificar Credenciales

### Abre DevTools (F12)

```
1. Presiona: F12
2. Ve a: Console
3. Busca errores rojos
```

### Errores comunes que verás:

```
❌ "Invalid API Key"
   └─ La apiKey no es correcta

❌ "projectId is not defined"
   └─ Falta el projectId

❌ "appId is not valid"
   └─ El appId no es válido

❌ "Failed to fetch"
   └─ Problema de conexión internet
```

---

## ✅ PASO 2: Verificar que Firebase está cargando

### En la Console (F12), ejecuta esto:

```javascript
// Copia y pega esto en la Console:
console.log('firebase:', typeof firebase);
console.log('firebase.app:', typeof firebase.app);
```

### Resultados esperados:

```
✅ CORRECTO:
   firebase: object
   firebase.app: function

❌ INCORRECTO:
   firebase: undefined
   firebase.app: undefined
   └─ Los scripts de Firebase NO cargaron
```

---

## 🔐 PASO 3: Verificar Credenciales Reales

### Abre tu Firebase Console

```
1. https://console.firebase.google.com
2. Tu proyecto: proveapp-35793
3. Configuración del proyecto (ícono de engranaje)
4. Aplicaciones web
5. Verifica estos valores:
```

### Copia exactamente estos 6 valores:

```javascript
const firebaseConfig = {
  apiKey: "AQUÍ_VA_EL_TUYO",              // ← Copia este
  authDomain: "AQUÍ_VA_EL_TUYO",          // ← Copia este
  projectId: "AQUÍ_VA_EL_TUYO",           // ← Copia este
  storageBucket: "AQUÍ_VA_EL_TUYO",       // ← Copia este
  messagingSenderId: "AQUÍ_VA_EL_TUYO",   // ← Copia este
  appId: "AQUÍ_VA_EL_TUYO"                // ← Copia este
};
```

---

## 🌐 PASO 4: Verificar que Firebase Console muestra datos

### Ir a Firebase Console

```
1. https://console.firebase.google.com
2. Tu proyecto
3. Firestore Database
4. ¿Ves una colección "proveedores"? 
   ✅ SÍ → Firebase existe
   ❌ NO → Crear la colección
```

---

## 📝 PASO 5: Crear colecciones manualmente

Si no existen, crearlas:

### Crear colección "proveedores":

```
1. Firebase Console
2. Firestore Database
3. Click: "Crear colección"
4. Nombre: proveedores
5. Click: "Siguiente"
6. Agrega un documento de prueba:
   - ID: auto
   - nombre: "Demo"
   - codigo: "DEMO123"
   - email: "demo@ejemplo.com"
7. Click: Guardar
```

### Crear colección "reservas":

```
1. Click: "Crear colección"
2. Nombre: reservas
3. Click: "Siguiente"
4. Agrega un documento de prueba:
   - ID: auto
   - codigoProveedor: "DEMO123"
   - fecha: "2024-12-20"
   - estado: "confirmada"
5. Click: Guardar
```

---

## 🔒 PASO 6: Verificar Firestore Rules

### Ir a Firestore → Rules

```
Las rules deben ser:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

### Si no están así:

```
1. Reemplaza TODO
2. Copia las rules de arriba
3. Click: "Publicar"
4. Espera a que se publique ✅
```

---

## 🧪 PASO 7: Test Manual en Console

### En DevTools Console (F12), prueba esto:

```javascript
// Verificar que firebase cargó
if (typeof firebase === 'undefined') {
    console.error('❌ Firebase no cargó');
} else {
    console.log('✅ Firebase cargó correctamente');
    
    // Obtener Firestore
    const db = firebase.firestore();
    console.log('✅ Firestore cargó');
    
    // Leer documento de prueba
    db.collection('proveedores').doc('test').get()
        .then(doc => {
            if (doc.exists) {
                console.log('✅ Firestore lee correctamente:', doc.data());
            } else {
                console.log('⚠️ Documento no existe (normal en first run)');
            }
        })
        .catch(error => {
            console.error('❌ Error leyendo Firestore:', error);
        });
}
```

---

## ✅ PASO 8: Verificar cada componente

### Haz esta prueba completa:

```javascript
// 1. ¿Firebase está disponible?
console.log('1. Firebase disponible:', typeof firebase !== 'undefined' ? '✅' : '❌');

// 2. ¿Tenemos firestore?
console.log('2. Firestore disponible:', typeof firebase.firestore !== 'undefined' ? '✅' : '❌');

// 3. ¿Podemos conectar a DB?
try {
    const db = firebase.firestore();
    console.log('3. Conexión a DB:', '✅');
} catch (e) {
    console.log('3. Conexión a DB:', '❌', e.message);
}

// 4. ¿Configuración correcta?
console.log('4. Config projectId:', firebase.app().options.projectId || '❌ No hay projectId');
```

---

## 🔴 Si SIGUE sin funcionar

### Elimina y vuelve a crear la app web

```
1. Firebase Console
2. Configuración del proyecto
3. Aplicaciones web
4. Busca tu app (proveapp-web)
5. Click en las 3 líneas "..."
6. Eliminar aplicación
7. Crear nueva aplicación web
8. Copia las credenciales NUEVAS
9. Reemplaza en el HTML
```

---

## 📊 CHECKLIST COMPLETO

```
[ ] Abrir DevTools (F12)
[ ] Ver si hay errores rojos
[ ] Ejecutar console.log('firebase:', typeof firebase)
[ ] Ver Firebase Console
[ ] Verificar credenciales correctas
[ ] Verificar colecciones existen
[ ] Verificar Firestore Rules publicadas
[ ] Ejecutar test manual en console
[ ] Si falla, eliminar y recrear app web
[ ] Probar de nuevo
```

---

## 🎯 CREDENCIALES CORRECTAS

### Tu proyecto DEBE tener:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyAMB9h7cNxaSiTbrjYfbHaA58QDgqbAayM",
  authDomain: "proveapp-35793.firebaseapp.com",
  projectId: "proveapp-35793",
  storageBucket: "proveapp-35793.firebasestorage.app",
  messagingSenderId: "734706343214",
  appId: "1:734706343214:web:c6acde37c1d491864a6ebf"
};
```

### Si son diferentes:

```
1. Ir a Firebase Console
2. Configuración proyecto
3. Copiar credenciales NUEVAS
4. Reemplazar en HTML
5. Guardar (Ctrl+S)
6. Recargar página (F5)
7. Intenta de nuevo
```

---

## 💡 TIPS

```
✅ Limpia caché: Ctrl+Shift+Del (Windows) o Cmd+Shift+Del (Mac)
✅ Recarga página: F5 o Ctrl+R
✅ Cierra DevTools: F12
✅ Abre DevTools de nuevo: F12
✅ Verifica internet: ¿conectado a WiFi?
✅ Prueba en navegador diferente
✅ Prueba incógnito/privado
```

---

## 🔍 ÚLTIMA VERIFICACIÓN

### En la URL de Firebase Console, verifica:

```
https://console.firebase.google.com/project/proveapp-35793/...

¿Dice "proveapp-35793"? 
✅ SÍ → Es el proyecto correcto
❌ NO → Estás en proyecto diferente
```

---

## 🆘 Si NADA funciona

### Opción 1: Crear proyecto nuevo

```
1. Firebase Console
2. Crear nuevo proyecto
3. Nombre: ProveApp-Nuevo
4. Crear Firestore
5. Copiar credenciales
6. Usar en app
```

### Opción 2: Contactar soporte Firebase

```
Firebase Support: https://firebase.google.com/support
Describe el error exacto de Console
```

---

## ✅ RESULTADO FINAL

Cuando funcione verás esto en Console:

```
✅ Firebase inicializado correctamente
✅ Firestore cargó
✅ Conexión a DB: ✅
✅ Config projectId: proveapp-35793
```

**Y la app debería funcionar normalmente.** 🎉

