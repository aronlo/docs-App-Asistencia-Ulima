# Capa de Dominio:

Esta es la capas más internas dentro de una arquitectura clean. Se componen tanto de los modelos como los casos de uso de la aplicación.

## Model

Dentro de los modelos (clases) que pueden ser usados por los casos de uso dentro de la aplicacion tenemos:

 - Attendee: es el asistente de un evento.
 - ChangeAttendee: se usa cuando a un asistente se le modifican sus datos, como por ejemplo marcarle la asistencia en un dia del evento o quiterla la asistencia.

```javascript
    //Hay que notar que cuando se instancia esta clase, se usa el primary key
    //para identificar a quien usuario cambiar sus datos, e ingresarlos los datos nuevos
    //como si fuera un JSON Object
    public ChangeAttendee(String key, JsonObject json) {
        this.key = key;
        this.json = json;
    }
```

## Usecase

Los casos de usos son una capa propia de la arquitectura clean. Cada caso de uso puede usar a su antojo las clases creadas para proporciarse a si mismo por ejemplo datos, como los repositorios.

La aplicación tiene los siguientes casos de usos, los cuales se explicarán brevemente:

 - **AddAttendee:** es el encargado de agregar un asistente a la base de datos.
```javascript
    //Tiene un método que recibe como parámetro el id del evento, un booleano que pregunta 
    public Flowable<Boolean> registerAttendee(int eventId, boolean fromFirebase, JsonObject attendee) {
        return dataRepository.addAttendee(eventId, fromFirebase, attendee);
    }    
```