# Capa de Presentación:

Esta capa se encarga de toda la presentación de los datos en la interfaz gráfica de la aplicación.

## Adapter

Son los "adaptadores" tipicos usados en android, así que no hay mucho de qué explicar. Se tratará de señalar **las cosas más importantes** para que entiendas el funcionamiento. Entre los que tenemos, encontramos:

### AttendeeRecyclerAdapter

Adaptador para el recyclerview usado en la pantalla de asistentes. La mayoría de las cosas estan indicadas en el mismo código, no obstante te comentaré algunas cosas claves que no están documentadas.

```javascript
    //Este lista contiene todos los asistentes de este evento y es usado por el adapter para mostrarlos en la ui
    private final List<AttendeeUI> attendeeUIList; 
    //Este hashMap actua como un diccionario en python si es que no lo sabes
    //Es un truco que se uso para poder saber la posicion de un asistente en la lista, dado su primarykey
    //El String --> Primary Key del asistente
    //Integer   --> La posicion del asistente en la lista
    private final Map<String, Integer> attendeeUIidIndexMap;
```

```javascript
    //Este método es llamado cada vez que le haces el swipe a un asistente
    @Override
    public void onItemDismiss(RecyclerView.ViewHolder viewHolder, int position, int direction) {
        attendeesActivity.hideKeyboard();
        //Obtienes el asistente que le fue hecho swipe
        AttendeeUI attendeeUI = attendeeUIList.get(position);
        //Valida si la fecha del celular está dentro de los dias del evento. 
        if(eventUI.getIsTodayAnEventDay()) {
            //Usa el caso de uso "editAttendace" para cambiarle la asistencia en firebase
            //PARECE CONFUSO, PERO TODO ESTO SON LOS PARÁMETROS DE UNA SOLA FUNCIÓN DEL CASO DE USO editAttendace
            compositeDisposable.add(editAttendance.editAttendance(
                eventUI.getId(), 
                eventUI.getFromFirebase(),
                attendeeUI.getKey(),
                eventUI.getToday(),
                //Este es el parámetro más importante porque cambia la asistencia de un dia a True o False
                !AttendeeUIUtil.isTrue(attendeeUI.getExtras().get("attendance"), eventUI.getToday()))
                .subscribe(aBoolean -> {
                    if(!aBoolean) attendeesActivity.showMessage("Aviso", R.string.message_network_local_cache);
                }, attendeesActivity.getAttendeesPresenter()::onError));
        } else {
            //Esto sirve indicarle al adapter que actualize la ui
            notifyDataSetChanged();
            attendeesActivity.showMessage("Error", R.string.message_not_event_day);
        }
    }
```

```javascript
    //Agrega un asistente al adapter
    public void addAttendeeUIToList(AttendeeUI attendeeUI) {
        synchronized (sharedLock) {
            attendeeUIList.add(attendeeUI);
            attendeeUIidIndexMap.put(attendeeUI.getId(), attendeeUIList.indexOf(attendeeUI));
        }
    }
```

```javascript
    //Actualiza un asistente del adapter
    //Por ejemplo cuando le marcas asistencia a un asistente, 
    //se tiene que hacer uso de esta función para ponerle el check verde
    public void updateAttendeeUI(AttendeeUI attendeeUI) {
        synchronized (sharedLock) {
            if(attendeeUIidIndexMap.get(attendeeUI.getId())!=null){
                attendeeUIList.set(attendeeUIidIndexMap.get(attendeeUI.getId()), attendeeUI);
            }else{
                addAttendeeUIToList(attendeeUI);
            }
        }
    }
```

```javascript
    //usado para limpiar un evento de todos sus asistentes
    public void clearAttendeeUIList() {
        synchronized (sharedLock) {
            attendeeUIList.clear();
            attendeeUIidIndexMap.clear();
        }
    }
```

### EventRecyclerAdapter
Adaptador para el recyclerview usado en la pantalla de eventos.

### PopupAttendeeRecyclerBinder
Adaptador para la pantalla de visualización de los datos de un asistente seleccionado.

---

Estos adaptadores son tipicas de la librería usada para el recycler view. Para mayor información, consultar este [link](https://github.com/satorufujiwara/recyclerview-binder).

### BinderAdapter
### BinderSection
### BinderViewType

## ItemView

Son elementos personalizos tales como: checkbox, radio buttons, etc. Son utilizados en la aplicación para la ui. Entre los cuales tenemos:

 - **BaseItemView:** es la clase abstracta que todas estas vistas deben de implementar.
 - **CheckBoxItemView**
 - **ChipItemView**
 - **DateItemView**
 - **CheckBoxItemView**
 - **MultipleChoiceListItemView**
 - **NumberItemView**
 - **RadioGroupItemView**
 - **SingleChoiceListItemView**
 - **TextItemView**

## Model

### Mapper

Son como binders, que tratan de convertir cierto elemento en otro para que puedan ser utilizados por otras clases.

 - **AttendeeUIToAttendeeMapper:** convierte objectos "Asistentes" en los objectos "Asistentes UI". Pues, el primero es el modelo que es almacenado en la base de datos y el otro es la clase que es utilizada para la UI. Cabe resaltar que se hace esto por utilizar la arquitectura **Clean**.
 - **EventUIToEventMapper** al igual que el anterior, convierte objectos "Events" a "Events UI" y viceversa.

Estos son los modelos que utilizan la UI:

 - **AttendeeUI**
 - **DetailPopupUI**
 - **EventUI**
 - **FieldUI:** estos son usados para saber que campos mostrar cada vez que se oprime un asistente. Por ejemplo: el email, el teléfono, etc.
 - **FilterUI:** es usado para mostrar un filtro de los asistentes. **ANTES HABíA ESTA OPCIÓN. SI ES QUE TE PREGUNTAS PORQUE NO LO ENCUENTRAS, ES QUE ESTÁ OCULTO ESE ELEMENTRO DE FILTRO.**

## Presenter

## Util

## View

## Widget

