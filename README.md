# EL PASE · Proyecto APK

Sugerencias de platos gourmet para restaurante, con investigación de tendencias,
escandallo, emplatado, impresión/PDF y biblioteca sincronizada en la nube.

- **Package:** `com.joseluengo.elpase`
- **Worker (fijo):** `https://comanda.luengof67.workers.dev/`
- **Firestore:** proyecto `hablemos-de-cocina`, colección `elpase_bibliotecas`

---

## Estructura

```
el-pase-apk/
├── www/
│   └── index.html          La app completa (Firebase + Worker ya configurados)
├── capacitor.config.json   com.joseluengo.elpase
├── package.json            Capacitor 6
├── firestore.rules         Reglas para pegar en la consola de Firebase
├── .github/workflows/
│   └── build.yml           Compila el APK firmado
└── README.md
```

---

## 1. Reglas de Firestore (una sola vez)

En la consola de Firebase → proyecto `hablemos-de-cocina` → **Firestore Database → Reglas**,
añade el bloque `match /elpase_bibliotecas/...` que hay en `firestore.rules`
**sin borrar** las reglas que ya tengas para el resto de la app. Publica.

## 2. Secrets de GitHub (una sola vez)

En el repositorio → Settings → Secrets and variables → Actions, crea los 4 de siempre:

| Secret | Contenido |
|--------|-----------|
| `KEYSTORE_BASE64` | tu keystore en base64 |
| `KEYSTORE_PASSWORD` | contraseña del keystore |
| `KEY_ALIAS` | alias de la clave |
| `KEY_PASSWORD` | contraseña de la clave |

Puedes reutilizar tu keystore de siempre. Para generar el base64:
`base64 -w0 tu-keystore.jks` (Linux) o `certutil -encode` (Windows).

## 3. Subir y compilar

```bash
git init
git add .
git commit -m "EL PASE v1"
git branch -M main
git remote add origin https://github.com/luengof67/el-pase.git
git push -u origin main
```

El workflow arranca solo con el push (o manualmente desde la pestaña **Actions**).
Al terminar, descarga el APK desde **Actions → build → Artifacts → el-pase-release**.

---

## Uso de la sincronización

La biblioteca se guarda **siempre en local** (funciona sin cobertura).
El botón **☁ Sincronizar** sube/baja de la nube usando un **código de sincronización**
que eliges una vez (p. ej. `jose-cocina-2026`). Usa el mismo código en tus dos PCs
y el móvil para ver la misma biblioteca en todos.

- La fusión es por `id`: gana la versión más reciente (`ts`).
- No borra en cascada: si quitas un plato en un equipo, seguirá en la nube hasta
  que pulses vaciar/sincronizar con criterio. (Si quieres borrado propagado, se añade.)

---

## Compilar en local (opcional)

```bash
npm install
npx cap add android
npx cap sync android
cd android
./gradlew assembleRelease
```

APK en `android/app/build/outputs/apk/release/`.
