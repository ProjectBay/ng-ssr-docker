1.  add new targets

```json
"server": {
    "builder": "@angular-devkit/build-angular:server",
    "options": {
    "outputPath": "dist/server",
    "main": "server.ts",
    "tsConfig": "tsconfig.server.json"
    },
    "configurations": {
    "production": {
        "outputHashing": "media"
    },
    "development": {
        "buildOptimizer": false,
        "optimization": false,
        "sourceMap": true,
        "extractLicenses": false,
        "vendorChunk": true
    }
    },
    "defaultConfiguration": "production"
},
"serve-ssr": {
    "builder": "@angular-devkit/build-angular:ssr-dev-server",
    "configurations": {
    "development": {
        "browserTarget": "{{projectName}}:build:development",
        "serverTarget": "{{projectName}}:server:development"
    },
    "production": {
        "browserTarget": "{{projectName}}:build:production",
        "serverTarget": "{{projectName}}:server:production"
    }
    },
    "defaultConfiguration": "development"
}
```

2. run `npx install browser-sync --save-dev`

3. add tsconfig.server.ts

```json
/* To learn more about this file see: https://angular.io/config/tsconfig. */
{
  "extends": "./tsconfig.app.json",
  "compilerOptions": {
    "outDir": "../../out-tsc/server",
    "target": "es2019",
    "types": ["node"]
  },
  "files": ["src/main.server.ts", "server.ts"]
}
```

4. In server.ts replace:

```typescript

11  const serverDistFolder = dirname(fileURLToPath(import.meta.url));
12  const browserDistFolder = resolve(serverDistFolder, '../browser');
13  const indexHtml = join(serverDistFolder, 'index.server.html');
```

with

```typescript
11  const distFolder = join(process.cwd(), 'dist/{{projectname}}/browser');
12  const indexHtml = existsSync(join(distFolder, 'index.original.html'))
13    ? join(distFolder, 'index.original.html')
14    : join(distFolder, 'index.html');
```

then:

- `server.set('views', browserDistFolder);` with `server.set('views', distFolder);`
- `express.static(browserDistFolder, {` with `express.static(distFolder, {`
- `documentFilePath: browserDistFolder,` with ` documentFilePath: indexHtml,`
- `publicPath: browserDistFolder,` with `publicPath: distFolder,`

then:

add `import 'zone.js/node';` as an import in the first line

5. Now you can run ```npx ng run {{projectName}}:serve-ssr

Docker now exposes the ports
