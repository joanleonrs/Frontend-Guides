#  Guía de Upgrade Angular:

##  Recomendaciones previas al upgrade:

**Node**

Debería haber instalado/actualizado la versión de Node **10.13** o posterior.

**Typescript**

Debería haber instalado/actualizado la versión de Typescript **3.7** o posterior.



## Tener en cuenta después de la migración

**Lazy Loading:**

Hay una nueva forma de carga diferida:

Ejemplo:

```javascript
// Antes
const routes: Routes = [{
  path: 'lazy',
  // La siguiente sintaxis para loadChildren ha sido deprecada
  loadChildren: './lazy/lazy.module#LazyModule'
}];

// Ahora
const routes: Routes = [{
  path: 'lazy',
  // La nueva sintaxis de import()
  loadChildren: () => import('./lazy/lazy.module').then(m => m.LazyModule)
}];
```


## Posibles errores en la ejecución del comando ng update

**Cleaning**

Si se producen errores por favor, elimine el archivo **package-lock.json** y la carpeta **node_modules**, luego de ello, ejecute el comando **npm install**.

Opcionalmente ejecute el comando **npm cache clean --force**.


**Error: El repositorio no está limpio**

En mi caso, tuve un error al decir que el repositorio no está limpio. Confirme u oculte los cambios antes de actualizar:

```javascript
ng update @angular/cli @angular/core --next  
```

Para resolverlo recomendamos ejecutar el comando adicionando el flag de **allow-dirty**:

```javascript
ng update @angular/cli @angular/core --next --allow-dirty 
```

**Instalación de Dependencias**

En Windows es necesario ejecutar **npm install** después **ng update @angular/cli @angular/core** para instalar las nuevas versiones de las dependencias.



##  Notas Opcionales:

**Material Design:**
En lugar de importar desde @angular/material, debe importar puntualmente el componente específico.

Ejemplo:

```javascript
// En vez de:
import {...} from '@angular/material';

// Se recomienda:
import {...} from '@angular/material/button';
```

**Ivy:**

Para trabajar con el compilador de Ivy, agregue lo siguiente en *tsconfig.json*:

```javascript
"angularCompilerOptions": { 
   "enableIvy": true 
}
```


## Para migración de Angular 7 a Angular 8:

**Static query migration**

Se requiere que todas las consultas **@ViewChild** y **@ContentChild** tengan un indicador 'estático' que especifique si la consulta es 'estática' o 'dinámica'. El compilador previamente clasificó las consultas automáticamente, pero en 8.0 los desarrolladores deben especificar explícitamente qué comportamiento se desea. Este es un requisito temporal como parte de una migración.

Ejemplo:

```javascript
// *Antes
// query results a veces disponible en `ngOnInit`, u otras veces en `ngAfterViewInit` (basado en template
@ViewChild('foo') foo: ElementRef;

// *Ahora
// query results disponible en ngOnInit
@ViewChild('foo', {static: true}) foo: ElementRef;

// OR

// query results disponible en ngAfterViewInit
@ViewChild('foo', {static: false}) foo: ElementRef;
```

**Tsconfig y tslint**

En tu **tsconfig.json** deberías cambiar lo siguiente opciones de compilación (compilerOptions):

-   El **module** value debe ser **esnext**.
-   El **target** value debe ser **es2015**.


**Polyfills**

En su polyfills.ts, la única línea necesaria que debe haber activado es:

Ejemplo:

```javascript
import 'zone.js/dist/zone';
```
