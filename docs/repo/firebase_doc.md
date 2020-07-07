# Estructura de firebase:

![silent_tears](./img/silent_tears.gif)

## Eventos
Cada evento debe de contener los siguientes campos para el correcto funcionamiento de la aplicación:
 - **attendeeTypes** *String[ ]*: este es un campo que es usado generalmente para poner el tipo de asistente que permite el evento.
 - **canAddAttendee** *Boolean*: este es un valor donde true hace aparecer un floating button en la esquina inferior derecha que permite agregar nuevos usuarios al evento; false hace que se oculte.
 - **color** *String*: este es un valor hexadeximal que permite configurar el color del evento.
 - **dataHidden** *String*: es una fecha en string que determina cuales de los eventos ocultar y que no se muestren en pantalla.
 - **dates** *String [ ]: contiene las fechas que permiten saber los días que cubrira el evento.
 - **detailsPopup** *String [ ]*: contiene los campos que se van a mostrar de los asistentes en en app, si es que se cliquea en uno. Casi siempre contiene los valores a mostrar del e-mail y el teléfono.
 - **fields** *HashMap<String; String>*: contiene la definición de los distintos campos que puede tener un asistente en su campo "extras".
 
 Este es un ejemplo de como sería si fuera un objeto JSON:
```javascript
    var fields = [
        {
            key: 'email',  //primary key
            name: 'Email', //Nombre a mostrar en el app
            type: 'text'   //tipo de dato
        },
        {
            key: 'phone',
            name: 'Teléfono', 
            type: 'text'
        }]
```

 - **fromFirebase** *Boolean*: indica que si es de Firebase o no. Por default, mantenerlo en true.
 - **id** *Integer*: primary key del evento.
 - **image** *String*: url de la imagen del evento.
 - **name** *String*: nombre del evento.

## Asistente
 Cada asistente debe de tener estos campos:
 - **id** *String*: es el identificador único del asistente. Generalmente es el DNI o un timestamp que se le asigna cuando se agrega un usuario sin DNI.
 - **name** *String*: nombre del asistente.
 - **extras** *HashMap<String; String>*: contiene los campos adicionales tales como:
    - **attendance** *HashMap<String; String>*: contiene las fechas que el usuario asistió. Si es false, no asistió a ese día.
    - **attendeeType** *String*: contiene el tipo de asistente que puede ser.
    - **email** *String*: contiene el e-mail.
    - **phone** *String*: contiene el teléfono.