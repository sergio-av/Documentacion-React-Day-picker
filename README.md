# React-Day-Picker

Esta diseñado para satisfacer las necesidades más basicas, a la hora de utilizar un selector de fecha en aplicaciones web. 

## Instalación

Vamos a ver dos formas de instalarlo para poder utilizarlo en nuestro proyecto, la primera es instalarlo como  **dependencia.**

Para ello vamos a utilizar los siguientes comandos en la terminal, situados en nuestro proyecto:

`$npm install react-day-picker --save`

O utilizando yarn

`$ yarn add react-day-picker` 

Despues solo quedaria importar el componente y su estilo:

```jsx
import DayPicker from 'react-day-picker';

import DayPickerInput from 'react-day-picker/DayPickerInput';

import 'react-day-picker/lib/style.css';
```

 Tambien podemos utilizar React-day-picker desde un **kg**

para esto debemos incuir el siguiente script:

```html
<script src="https://unpkg.com/react-day-picker/lib/daypicker.min.js"></script>
<link rel="stylesheet" href="https://unpkg.com/react-day-picker/lib/style.css">
```

Una vez esto tendremos los componentes disponibles como `window.DayPicker` y `window.DayPicker.Input`



**Ejemplo de como seria**



[![Captura.jpg](https://i.postimg.cc/0N1nzHbH/Captura.jpg)](https://postimg.cc/FYG3qpbj)

Y el codigo seria tal que así 

```jsx
import React from 'react';
import DayPicker from 'react-day-picker';
import 'react-day-picker/lib/style.css';

export default function Hello() {
  return <DayPicker />;
}
```



## Conceptos básicos

Lo primero es mencionar que React-day-picker de forma predeterminada mostrara en el calendario el mes actual.

Ademas de marcarnos el dia en el que estamos hoy, mientras que el usuario interactua de forma normal con el calendario, como podemos observar en la siguiente imagen.

**Ejemplo de como seria**

[![dia-Seleccionado.jpg](https://i.postimg.cc/j54s75tn/dia-Seleccionado.jpg)](https://postimg.cc/7Gb8vqCq)

El codigo que genera este calendario es el siguiente

```jsx
import React from 'react';
import DayPicker from 'react-day-picker';

import 'react-day-picker/lib/style.css';

export default class BasicConcepts extends React.Component {
  constructor(props) {
    super(props);
    this.handleDayClick = this.handleDayClick.bind(this);
    this.state = {
      selectedDay: undefined,
    };
  }

  handleDayClick(day) {
    this.setState({ selectedDay: day });
  }

  render() {
    return (
      <div>
        <DayPicker onDayClick={this.handleDayClick} /> <--Destacamos esta linea
        {this.state.selectedDay ? (
          <p>You clicked {this.state.selectedDay.toLocaleDateString()}</p>
        ) : (
          <p>Please select a day.</p>
        )}
      </div>
    );
  }
}
```

para cambiar esto y que a la hora de nosotros hacer click en un dia , se nos remarque en el calendario el dia seleccionado, solo debemos de añadirle una propiedad al componente DayPicker. 

```jsx
render() {
    return (
      <div>
        <DayPicker
          onDayClick={this.handleDayClick}
          selectedDays={this.state.selectedDay} // Aqui
        />
        {this.state.selectedDay ? (
          <p>You clicked {this.state.selectedDay.toLocaleDateString()}</p>
        ) : (
          <p>Please select a day.</p>
        )}
      </div>
    );
  }
```

Esto se debe a que React-date-day tabraja con los estados de los dias del calendario por eso podemos trabajar con el resto del calendario en el anterior ejemplo sin que el estado del dia puesto como predefinido, se vea afectado.

**Ejemplo de como seria**

[![nuevodia-Seleccionado.jpg](https://i.postimg.cc/vHTkdBg8/nuevodia-Seleccionado.jpg)](https://postimg.cc/svFwY34b)

Muy bien ahora continuamos para deseleccionar un dia marcado, para esto solo debemos de hacer un click en el dia que estaba seleccionado, pero para que esto funciones debemos de añadir las siguientes lineas de codigo , a la funcion handleDayClick 

```jsx
 handleDayClick(day, { selected }) {
    if (selected) {
      // deseleccionar el dia seleccionado
      this.setState({ selectedDay: undefined });
      return;
    }
```

Otra funcion interesante es la desabilitacion de dias de esta forma el usuario no podra selecionar ningun dia de los que hemos seleccionado.  esto lo realizamos mediante la props disabledDays, en esta debemos de seleccionar el dia que queremos deshabilitar de la siguiente forma: 

```jsx
import React from 'react';
import DayPicker from 'react-day-picker';

import 'react-day-picker/lib/style.css';

export default class BasicConcepts extends React.Component {
  render() {
    return (
      <div>
        <DayPicker disabledDays={{ daysOfWeek: [0] }} /> // Aqui
      </div>
    );
  }
}
```

A demas de añadir la condicion de que si el dia se encuentra deshabilitado el usuario no pueda remarcarlo. seria tal que asi 

```jsx
handleDayClick(day, { selected, disabled }) {
    if (disabled) {
      // si el dia esta desactivado no hagas nada
      return;
    }
    if (selected) {
      // y si el dia esta seleccionado deseleccionalo
      this.setState({ selectedDay: undefined });
      return;
    }
    this.setState({ selectedDay: day });
  }
```

[![deseleccion.jpg](https://i.postimg.cc/ryxBtfXr/deseleccion.jpg)](https://postimg.cc/4n4LDb3f) 

## Estilo

Para dar estilo a React-day-picker lo primero que deberemos de hacer sera importar la plantilla de estilo esto lo podemos hacer la siguientes formas: 

* Importando la hoja de estilo desde sus archivos Sass como por ejemplo de node_module:

```css
@import "../node_modules/react-day-picker/lib/style"
```

* Importando directamente en el js:

  ```jsx
  import "react-day-picker/lib/style.css";
  ```

* O tambien desde Unkg:

  ```html
  <link rel="stylesheet" href="https://unpkg.com/react-day-picker/lib/style.css">
  ```

Una vez realizado esto ya podriamos empezar a retocar el estilo del calendario.



### **Modificadores de estilo**

El estilo de React-day-picker funciona a traves de modificadores es decir debemos de añadir la prop `modifiers` en la que se especifica el elemento que deseamos estilizar.

**3 tipos de ejemplos**

* **Estilos en la misma lineas de codigo**

  Utilizando modifiersSTyles

```jsx
import React from 'react';
import DayPicker from 'react-day-picker';
import 'react-day-picker/lib/style.css';

export default function StylingInline() {
  const modifiers = {
    thursdays: { daysOfWeek: [4] },
    birthday: new Date(2018, 9, 30),
  };
  const modifiersStyles = {
    birthday: {
      color: 'white',
      backgroundColor: '#ffc107',
    },
    thursdays: {
      color: '#ffc107',
      backgroundColor: '#fffdee',
    },
  };
  return (
    <DayPicker
      month={new Date(2018, 9)}
      modifiers={modifiers}
      modifiersStyles={modifiersStyles}
    />
  );
}
```

**Ejemplo**

[![imagen-2021-03-08-154547.png](https://i.postimg.cc/BvXwVnVd/imagen-2021-03-08-154547.png)](https://postimg.cc/sBCcBsyc)

* **Estilo con modulos CSS**

  Una vez  tengas el sistema configurado realizamos la importacion desde el CSS Modules, despues al componente deseado le damos la props className = { styles } para que esta reciba las estilización del css. 

  ```jsx
  import React from 'react';
  import DayPicker from 'react-day-picker';
  
  import styles from '../styles/cssmodules.css';
  
  export default function CSSModules() {
    return <DayPicker classNames={ styles } />
  }
  ```



* **Estilo con modulos CSS y aplicando Modifiers**

  Esta forma no hace otra cosa que combinar los dos metodos anteriores de aplicar estilo.

  ```jsx
  import React from 'react';
  import DayPicker from 'react-day-picker';
  import styles from '../styles/cssmodules.css';
  
  export default function CSSModules() {
    return (
      <DayPicker 
        classNames={ styles } 
        modifiers={{
          [styles.birthday]: new Date(2018, 8, 19)
        }}
      />
    );
  }
  ```



## Localización 

React-day-picker puede ser localizado en cualquier idioma, de forma predeterminada viene el ingles. Para realizar esto seria muy sencillo deberiamos editar las constantes que ya vienen señaladas como:

- **MONTHS** Para añadir los meses con su nombre completo.
- **WEEKDAYS_LONG** Para los nombres de los dias de forma completa.
- **WEEKDAYS_SHORT** Para los nombres de los dias de forma abreviada.



```jsx
import React from 'react';
import DayPicker from 'react-day-picker';
import 'react-day-picker/lib/style.css';

const MONTHS = [
  'Gennaio',
  'Febbraio',
  'Marzo',
  'Aprile',
  'Maggio',
  'Giugno',
  'Luglio',
  'Agosto',
  'Settembre',
  'Ottobre',
  'Novembre',
  'Dicembre',
];
const WEEKDAYS_LONG = [
  'Domenica',
  'Lunedì',
  'Martedì',
  'Mercoledì',
  'Giovedì',
  'Venerdì',
  'Sabato',
];
const WEEKDAYS_SHORT = ['Do', 'Lu', 'Ma', 'Me', 'Gi', 'Ve', 'Sa'];

export default function Italian() {
  return (
    <DayPicker
      locale="it"
      months={MONTHS}
      weekdaysLong={WEEKDAYS_LONG}
      weekdaysShort={WEEKDAYS_SHORT}
      firstDayOfWeek={1}
    />
  );
}
```

[![calendario-Ita.jpg](https://i.postimg.cc/Tw4ycGs2/calendario-Ita.jpg)](https://postimg.cc/wyDTqKHn) 



## DayPickerInput

React-day-picker trae tambien un componente llamado `<DayPickerInput/>`, este componente lo que hace crear un boton en el que tu escribes mediante un formato de fecha: Año - Mes - Dia

para utilizarlo solo hay que realizar la siguiente importacion.

```jsx
import DayPickerInput from 'react-day-picker/DayPickerInput';
```



**Ejemplo**

```jsx
import React from 'react';
import DayPickerInput from 'react-day-picker/DayPickerInput';
import 'react-day-picker/lib/style.css';

export default class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleDayChange = this.handleDayChange.bind(this);
    this.state = {
      selectedDay: undefined,
    };
  }

  handleDayChange(day) {
    this.setState({ selectedDay: day });
  }

  render() {
    const { selectedDay } = this.state;
    return (
      <div>
        {selectedDay && <p>Day: {selectedDay.toLocaleDateString()}</p>}
        {!selectedDay && <p>Choose a day</p>}
        <DayPickerInput onDayChange={this.handleDayChange} />
      </div>
    );
  }
}
```

[![image-20210308161906044](C:\Users\Usuario\AppData\Roaming\Typora\typora-user-images\image-20210308161906044.png) 

## Conclusión

En terminos generales react-day-picker es ahora mismo el segundo componente de calendario mas potento,

ya sea por su facilidez para la instalacion, la facil comprension de su sintaxis y lo comodo que es aplicarle estilos. Lo convierten en el segundo mas descargado lo podemos ver en esta gráfica:

[![imagen-2021-03-08-163121.png](https://i.postimg.cc/PrZV7Jrv/imagen-2021-03-08-163121.png)](https://postimg.cc/PPXMptDd) 

