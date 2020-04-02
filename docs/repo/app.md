# App:

Básicamente es lo que se ejecuta al inicio de toda aplicación e inicializa componentes tales como el Firebase y la base de datos local Realm.

```javascript
    public class App extends DaggerApplication{

        @Override
        public void onCreate() {
            super.onCreate();
            FirebaseDatabase.getInstance().setPersistenceEnabled(true);
            Realm.init(this);
        }

        @Override
        protected AndroidInjector<? extends DaggerApplication> applicationInjector() {
            //Esto se coloca por dagger,  para incluir el context en el grafo de
            //la app y cualquier clase que la requiera, pueda hacer uso de ella
            return DaggerAppComponent.builder().create(this);
        }
    }
```