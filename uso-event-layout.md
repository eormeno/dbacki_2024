# Taller: Uso de Event Layout

Event Layout es un sistema de plantillas de Laravel que permite personalizar la apariencia de las páginas web. En este taller, aprenderás a utilizar Event Layout para crear un diseño personalizado para tu aplicación web.
Este layout incorpora un sistema de notificaciones toast que permite mostrar mensajes emergentes en la pantalla. Además, incluye estilos predefinidos para los toasts de información, éxito y error.

## Instrucciones
Sigue los pasos a continuación para completar el taller:

1. Realiza los cambios necesarios en el controlador `ContadorController` para mostrar notificaciones toast al incrementar y decrementar el contador.
2. Implementa un nuevo método en el Trait `ToastTrigger` para mostrar una alerta con una duración extendida.
3. Actualiza las vistas `contador.blade.php` y `layouts/event.blade.php` para utilizar el nuevo layout y los estilos de toasts.
4. Modifica el script de inicio `start.ps1` para incluir una opción de reinicio de la base de datos.

## 1. Cambios en ContadorController

### Adición del Trait ToastTrigger
```php
use App\Traits\ToastTrigger;
// ...
class ContadorController extends Controller
{
    use ToastTrigger;
```
Se ha añadido el trait `ToastTrigger` al controlador para habilitar la funcionalidad de notificaciones toast.

### Modificaciones en los métodos
- **incrementar()**: Ahora muestra una alerta
  ```php
  $this->alert('Precaución: número incrementado');
  ```
- **decrementar()**: Ahora muestra un toast de error
  ```php
  $this->errorToast('Número decrementado');
  ```

## 2. Mejoras en ToastTrigger Trait

Se ha añadido un nuevo método `alert()`:
```php
public function alert(string $message) {
    $this->_toast($message, 100000, "alert");
}
```
Este método crea un toast de tipo alerta con una duración extendida de 100,000 (presumiblemente milisegundos).

## 3. Cambios en las Vistas

### Actualización de contador.blade.php
- Cambio de layout: de `x-app-layout` a `x-event-layout`
- Cambio de slot: de `header` a `title`

```html	
<x-event-layout>
    <x-slot name="title">Contador</x-slot>
    <!-- Contenido de la página -->
</x-event-layout>
```

### Mejoras en layouts/event.blade.php
Se han añadido nuevos componentes toast:
1. **Toast de Alerta**
   ```html
   <x-toast name="alert">
       <div class="bg-slate-400 shadow-lg p-6 border border-red-400 rounded-md">
           <x-toast-message />
           <x-button class="bg-cyan-500 hover:bg-cyan-600 mt-4"
               onclick="document.getElementById('toast-alert').style.display = 'none'">
               Cerrar
           </x-button>
       </div>
   </x-toast>
   ```
2. Se han ajustado los estilos de los toasts existentes:
   - Info: cambio de color de fondo a `bg-cyan-600`
   - Success: cambio a `bg-green-700`
   - Error: cambio a `bg-red-700`

## 4. Cambios en el Script de Inicio

Se ha actualizado `start.ps1` para incluir una opción de reinicio de base de datos:
```powershell
if ($args -contains '-r') {
    Remove-Item database/database.sqlite -ErrorAction SilentlyContinue
    php artisan migrate --force --seed
}
```

## Conclusión
Los cambios se centran en mejorar el sistema de notificaciones, añadiendo un nuevo tipo de alerta y refinando los estilos de los toasts existentes. También se han realizado mejoras en la estructura del layout y se ha añadido funcionalidad para reiniciar la base de datos durante el desarrollo.