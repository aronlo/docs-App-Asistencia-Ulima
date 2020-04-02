# Injección de Dependencias:

Aca te dejo una pequeña definición por si no sabes de que estoy hablando.

> En informática, inyección de dependencias (en inglés Dependency Injection, DI) es un patrón de diseño orientado a objetos, en el que se suministran objetos a una clase en lugar de ser la propia clase la que cree dichos objetos. Esos objetos cumplen contratos que necesitan nuestras clases para poder funcionar (de ahí el concepto de dependencia). Nuestras clases no crean los objetos que necesitan, sino que se los suministra otra clase 'contenedora' que inyectará la implementación deseada a nuestro contrato.


En esta sección, no se explicará de manera detallada como funciona la inyección de dependencias. Si quieres aprender más sobre esto, puedes ver los links que deje al comienzo.

Simplemente se hara una simple mención de los módulos y componentes que se usaron para este proyecto.

## Component:

Permite definir y crear el grafo de dependencias de la aplicación.

```javascript
    @Component(modules = {
            AppModule.class,
            DataModule.class,
            BuildersModule.class,
            BindingModule.class,
            AndroidSupportInjectionModule.class
    })

    public interface AppComponent extends AndroidInjector<App>{
        @Component.Builder
        abstract class Builder extends AndroidInjector.Builder<App> {
        }
    }
```

## Module

Contiene los distintos "modulos" separados semánticamente por la función que desempeñan dentro de la aplicación.

### AppModule

Provee objectos de uso general que puedan ser usados por cualquier clase dentro de la app, como el contexto, gson, etc.

### BindingModule

Enlaza las views que requieren los presenters de cada activitidad con sus respectivas actividades. Previamente estas actividades deben de implementar las vistas que requieran las presenters.

### BuildersModule

Provee de los "Activities" que requieran los presenters, controlers o cualquier otra clase.

### DataModule

Permite proveer a las clases que la necesitan, objetos relacionados a consulta de servicios en la red, tales como retrofit, okhttpclient, implementaciones de distintos servicios, etc.