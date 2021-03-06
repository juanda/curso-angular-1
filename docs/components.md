# Components

Probablemente el elemento más importante en toda la arquitectura de Angular sea el componente. El concepto de componente es bien conocido en la ingeniería del software y lleva asociado como principal idea la de ser un elemento reutilizable.

En el mundo web, desde hace algún tiempo, existe una especificación denominada Web Compontents (https://www.w3.org/wiki/WebComponents/, https://www.webcomponents.org/introduction),que propone una solución basada en componentes para el desarrollo de aplicaciones web.

Los componentes de Angular son muy similares a los componentes de la especificación Web Component. Incluso existen herramientas para convertir los componentes de Angular en Web Components y también se pueden usar Web Components en aplicaciones Angular.


Un componente controla un trozo de la pantalla llamado *vista* (*view*).

Por ejemplo, en un momento concreto, en el navegador:

- El componente raíz habría dibujado los links de navegación o menú de la aplicación.

- Otro componente podría estar dibujando un listado ordenado y paginado de elementos de nuestro negocio.

- Otro compenente habría pintado un calendario del mes actual en el que están resaltados los días en los que el usuario tiene algún evento.

Angular crea, actualiza y destruye componentes mientras el usuario se mueve por la aplicación.


Ejemplo de componente:

```typescript
@Component({
  selector:    'events-list',
  templateUrl: './events-list.component.html',
  providers:  [ EventsService ]
})
export class EventsListComponent implements OnInit {
/* . . . */
}
```

Un componente es una clase que implementa el método OnInit y decorada con el decorador @Component.

La función decoradora @Component recibe un Objeto con las siguientes propiedades:

- **selector:** Es el selector CSS que se utiliza en las plantillas para indicar a Angular donde debe crear e insertar una instancia del componente. En nuestro ejemplo, cada vez que angular encuentre la etiqueta &lt;events-list>&lt;/events-list>, insertará una instacia de la vista de EventsListComponent en esa etiqueta.
- **template:** El código HTML del template de este componente.
- **templateUrl:** La ruta relativa al componente del archivo donde se encuenta el HTML del template de este componente. Si se utiliza *templateUrl* NO se utiliza *template*
- **providers:** array de proveedores de inyección de dependecias (dependency injection providers) para servicios que este componente necesite. Es una de las maneras de informar a Angular que el constructor de este componente necesita una instancia de un servicio concreto. No obstante durante el curso utilizaremos inyección de dependencias directamente en los constructores del componente.
- **styles:** array de strings con estilos CSS exclusivos para este componente.
- **styleUrls:** array de ficheros css exclusivos para este componente.

El template, los metadatos y el compenente, en conjunto, describen una **vista**.

## Creación de un componente con angular cli

```
ng generate component nombre-componente
```

o bien 

```
ng g c nombre-componente
```

```
installing component
  create src/app/nombre-componente/nombre-componente.component.css
  create src/app/nombre-componente/nombre-componente.component.html
  create src/app/nombre-componente/nombre-componente.component.spec.ts
  create src/app/nombre-componente/nombre-componente.component.ts
  update src/app/app.module.ts
```

Ejercicio: Crear dos componentes distintos e insertarlos dentro de la vista/componente raíz.

Ejercicio: Insertar uno de los componentes dentro del otro.


## CSS en los componentes

Los estilos especificados en los metadatos de un componente, solamente aplican a dicho componente.

Hay varias formas de añadir estilos a un componente:

- Mediante las propiedades styles y/o styleUrls de los metadatos del componente.
- En el propio código HTML del componente.
- Con CSS imports.

### Estilos en la propiedad *styles*

```typescript
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }','h2 { color: red; }']
})
export class HeroAppComponent {
/* . . . */
}
```

Angular CLI crea la propiedad *styles* vacía si generamos el componente con el modificador *--inline-style*:

> ng generate component hero-app --inline-style

### Estilos en la propiedad *styleUrls*

```typescript
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styleUrls: ['./hero-app.component.css']
})
export class HeroAppComponent {
/* . . . */
}
```

Angular CLI crea un fichero .css vacío y una propiedad styleUrls si se crea el componente sin modificador de estilo:

> ng generate component hero-app

Las propiedades styles y styleUrls pueden coexistir en el mismo componente sin ningún problema.

### Template inline styles

```typescript
@Component({
  selector: 'app-hero-controls',
  template: `
    <style>
      button {
        background-color: white;
        border: 1px solid #777;
      }
    </style>
    <h3>Controls</h3>
    <button (click)="activate()">Activate</button>
  `
})
```

o 

```typescript
@Component({
  selector: 'app-hero-team',
  template: `
    <link rel="stylesheet" href="assets/hero-team.component.css">
    <h3>Team</h3>
    <ul>
      <li *ngFor="let member of hero.team">
        {{member}}
      </li>
    </ul>`
})
```

NOTA: El link es relativo al *application root*.

Las CSS indicadas en el HTML NO son exclusivas de este componente.

### CSS @imports

Dentro de un archivo css, se pueden importar otros archivos css mediante la regla @import propia del lenguaje CSS:

```css
@import 'hero-details-box.css';
```

Las CSS importadas mediante la regla @import NO son exclusivas del componente.

### Archivos .scss, .less y .styl

Si el proyecto está configurado para trabajar con .scss, .less o .styl, aplican las mismas normas.

```typescript
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
```

En el proceso de compilación, angular compilará las CSS automáticamente.

## Shadow DOM

Angular es un framework web orientado a componentes. Existe una especificación oficial del consorcio W3 denominada Web Components (https://www.w3.org/TR/components-intro/) y angular utiliza el mismo concepto para sus componente. Incluso se pueden exportar los componentes de angular como elementos de web components (https://medium.com/vincent-ogloblinsky/export-angular-components-as-custom-elements-with-angular-elements-a2a0bfcd7f8a)

Shadow DOM forma parte de la especificación Web Components. Es una forma de establecer contornos funcionales entre distintos árboles DOM. Proporciona una encapsulación del DOM dentro del DOM. Con Shadow DOM se consigue una encapsulación verdadera con componentes scoped, es decir cada componente define su propio DOM de manera que, por ejemplo, las clases CSS’s y el código javascript que se defina en un componente  afecta sólo a dicho componente.

Los componentes de angular se comportan según este principio. Como no todos los navegadores soportan aún la especificación Web Components y Shadow DOM, por defecto angular realiza la encapsulación de los componentes mediante emulación. No obstante se puede indicar explícitamente que se use Shadow DOM nativo.

```javascript

@Component({
 selector: 'app-naranja',
 encapsulation: ViewEncapsulation.Native,
 templateUrl: './naranja.component.html',
 styleUrls: ['./naranja.component.css']
})
```

El siguiente artículo explica bastante bien el concepto de shadow dom.

https://toddmotto.com/web-components-concepts-shadow-dom-imports-templates-custom-elements/

Y este otro la relación de shadow dom com angular >2.

https://toddmotto.com/emulated-native-shadow-dom-angular-2-view-encapsulation



[Índice](index.md)
