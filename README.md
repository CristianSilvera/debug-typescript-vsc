# debug-typescript-vsc

```
$ npm init -y

```
Instalo typescript de forma local
```
$ npm install -D typescript

```

Creo una carpeta src e inicio un proyecto de typescript

```
$ npx tsc --init

```

##Modificaciones en tsconfig.json

Modificar el directorio de ./ a ./src:

```
/* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    "rootDir": "./src",                                  /* Specify the root folder within your source files. */
    // "moduleResolution": "node10",
```
Habilitar sourceMap: true

Modificar la ruta de outDir:

```
    "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */
    // "inlineSourceMap": true,                          /* Include sourcemap files inside the emitted JavaScript. */
    // "noEmit": true,                                   /* Disable emitting files from a compilation. */
    // "outFile": "./",                                  /* Specify a file that bundles all outputs into one JavaScript file. If 'declaration' is true, also designates a file that bundles all .d.ts output. */
    "outDir": "./dist",                                   /* Specify an output folder     
```

## Posibles problemas:

Al ejecutar comandos, se inicia automáticamente el depurador de Node.js.

### Solución: 
`--inspect`, desactívalo temporalmente con:
```sh
unset NODE_OPTIONS
```

Si no encontraste ningún archivo con la configuración de `--inspect`, prueba los siguientes pasos en orden para eliminar cualquier configuración persistente:

---

### **1. Deshabilitar NODE_OPTIONS en la Terminal**
Ejecuta este comando en Git Bash o en la terminal de VSCode para verificar si `NODE_OPTIONS` está activo:
```sh
echo $NODE_OPTIONS
```
Si ves algo como `--inspect`, desactívalo temporalmente con:
```sh
unset NODE_OPTIONS
```
Para deshabilitarlo permanentemente en Git Bash (MINGW64), edita el archivo `~/.bashrc` o `~/.bash_profile` y elimina cualquier línea que contenga:
```sh
export NODE_OPTIONS="--inspect"
```
Luego, reinicia la terminal o ejecuta:
```sh
source ~/.bashrc
```

---

### **2. Ejecutar Node sin Debugger**
Para probar si el problema es persistente, intenta ejecutar Node con `--no-inspect`:
```sh
node --no-inspect src/index.ts
```
Si esto funciona sin activar el depurador, es una señal de que `NODE_OPTIONS` estaba configurado.

---

### **3. Revisar la configuración de VSCode**
Si el problema persiste, revisa la configuración de VSCode:

1. **Abrir la configuración de VSCode (`settings.json`)**
   - Presiona `Ctrl + Shift + P` y busca `Preferences: Open Settings (JSON)`.
   - Busca cualquier línea con `--inspect` o `NODE_OPTIONS`.

2. **Eliminar configuración de depuración en `launch.json`**
   - Abre la carpeta `.vscode/` en tu proyecto.
   - Si existe `launch.json`, revisa si hay algo como:
     ```json
     "runtimeArgs": ["--inspect"]
     ```
   - Si lo encuentras, elimínalo o cambia `--inspect` por `--no-inspect`.

3. **Restablecer la configuración de VSCode**
   Si nada funciona, puedes restablecer la configuración de depuración:
   - Ve a **Ajustes de VSCode** (`Ctrl + ,`).
   - Busca `debug.node.autoAttach`.
   - Desactívalo si está activado.

---

### **4. Revisar si `nodemon` está ejecutando `--inspect`**
Si en algún momento usaste `nodemon`, revisa el archivo `nodemon.json` en tu proyecto:
```sh
cat nodemon.json
```
Si ves:
```json
{
  "execMap": {
    "js": "node --inspect"
  }
}
```
Cámbialo a:
```json
{
  "execMap": {
    "js": "node"
  }
}
```

---

### **5. Probar en una Nueva Terminal**
Cierra todas las instancias de la terminal y abre una nueva. Luego, prueba ejecutando:
```sh
node src/index.ts
```
Si sigue mostrando `Debugger listening...`, reinicia VSCode y vuelve a probar.

---

### **6. Verificar Configuración Global de Node.js**
Ejecuta:
```sh
node -p "process.env"
```
Esto imprimirá todas las variables de entorno. Si ves algo relacionado con `NODE_OPTIONS="--inspect"`, revisa los archivos de configuración de tu sistema:
- En Linux/macOS: `~/.bashrc`, `~/.profile`, `~/.zshrc`
- En Windows (Git Bash): `~/.bashrc`, `~/.bash_profile`
- En Windows (PowerShell o CMD): Ejecuta `SystemPropertiesAdvanced` → Variables de entorno.

Si `NODE_OPTIONS` aparece en las variables de entorno del sistema, elimínalo.

---





Formas de ejecución transpilación: 

```
$ npx tsc

```
Podemos utilizar sucrase
```
$ npm i -D sucrase
```

# Debugging TypeScript

https://code.visualstudio.com/docs/typescript/typescript-debugging
```

{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/helloworld.ts",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": ["${workspaceFolder}/out/**/*.js"]
    }
  ]
}
```
Agrega esta línea al archivo launch.json:

```
"preLaunchTask": "tsc: build - tsconfig.json",
```
