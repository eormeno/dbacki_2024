### Extensión de la Funcionalidad del Método `failedValidation()` en Laravel

En este tutorial, vamos a explicar cómo extender la funcionalidad del método `failedValidation()` en Laravel para que los errores de validación sean enviados a un mecanismo de notificación tipo "toast". También veremos cómo manejar la validación de la actualización de una imagen, donde esta no es requerida.

#### Paso 1: Crear la Clase de Solicitud

Laravel proporciona una forma conveniente de validar las solicitudes HTTP a través de clases de solicitud personalizadas. En este ejemplo, tenemos dos clases de solicitud: [`StoreActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FStoreActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A8%2C%22character%22%3A6%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition") y [`UpdateActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FUpdateActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A6%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition").

#### Paso 2: Definir las Reglas de Validación

En ambas clases, definimos las reglas de validación en el método `rules()`. Estas reglas aseguran que los datos enviados en la solicitud cumplan con ciertos criterios.

```php
public function rules(): array
{
    return [
        'name' => ['required', 'string', 'max:255'],
        'description' => ['required', 'string'],
        'image' => ['required', 'image', 'mimes:jpeg,png,jpg,gif,svg', 'max:2048'],
    ];
}
```

En [`UpdateActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FUpdateActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A6%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition"), la regla para la imagen no incluye [`required`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FStoreActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A28%2C%22character%22%3A24%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FUpdateActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A25%2C%22character%22%3A24%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition") porque el usuario puede optar por no actualizar la imagen.

```php
public function rules(): array
{
    return [
        'name' => ['required', 'string', 'max:255'],
        'description' => ['required', 'string'],
        'image' => ['image', 'mimes:jpeg,png,jpg,gif,svg', 'max:2048'],
    ];
}
```

#### Paso 3: Personalizar los Mensajes de Error

Podemos personalizar los mensajes de error en el método `messages()`.

```php
public function messages(): array
{
    return [
        'name.required' => 'El nombre es requerido',
        'name.string' => 'El nombre debe ser una cadena de texto',
        'name.max' => 'El nombre no debe exceder los 255 caracteres',
        'description.required' => 'La descripción es requerida',
        'description.string' => 'La descripción debe ser una cadena de texto',
        'image.required' => 'La imagen es requerida',
        'image.image' => 'La imagen debe ser un archivo de imagen',
        'image.mimes' => 'La imagen debe ser de tipo: jpeg, png, jpg, gif, svg',
        'image.max' => 'La imagen no debe exceder los 2048 kilobytes',
    ];
}
```

#### Paso 4: Extender el Método `failedValidation()`

Para enviar los errores de validación a un mecanismo de notificación tipo "toast", extendemos el método `failedValidation()`.

```php
protected function failedValidation(Validator $validator)
{
    $this->errorToast($validator->errors()->first());
    parent::failedValidation($validator);
}
```

En este método, llamamos a `$this->errorToast()` con el primer mensaje de error. Luego, llamamos al método `failedValidation()` del padre para asegurarnos de que el flujo de validación de Laravel continúe como de costumbre.

#### Paso 5: Implementar el Método `errorToast()`

El método `errorToast()` se puede implementar en un trait llamado [`ToastTrigger`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FStoreActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A15%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition") que se incluye en la clase de solicitud.

```php
trait ToastTrigger
{
    public function errorToast($message)
    {
        // Implementación para mostrar un toast con el mensaje de error
    }
}
```

#### Paso 6: Incluir el Trait en la Clase de Solicitud

Finalmente, incluimos el trait en la clase de solicitud.

```php
use App\Traits\ToastTrigger;

class StoreActivityRequest extends FormRequest
{
    use ToastTrigger;
    // ...
}
```

### Validación de la Actualización de Imagen

En [`UpdateActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FUpdateActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A6%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition"), la imagen no es requerida porque el usuario puede optar por no actualizar la imagen actual. Esto se refleja en las reglas de validación.

```php
public function rules(): array
{
    return [
        'name' => ['required', 'string', 'max:255'],
        'description' => ['required', 'string'],
        'image' => ['image', 'mimes:jpeg,png,jpg,gif,svg', 'max:2048'],
    ];
}
```

### Conclusión

En este tutorial, hemos visto cómo extender la funcionalidad del método `failedValidation()` en Laravel para enviar errores de validación a un mecanismo de notificación tipo "toast". También hemos explicado cómo manejar la validación de la actualización de una imagen, donde esta no es requerida.

#### Placeholder para Capturas de Pantalla

- **Captura de pantalla 1:** Definición de reglas de validación en [`StoreActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FStoreActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A8%2C%22character%22%3A6%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition").
- **Captura de pantalla 2:** Definición de reglas de validación en [`UpdateActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FUpdateActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A6%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition").
- **Captura de pantalla 3:** Implementación del método `failedValidation()`.
- **Captura de pantalla 4:** Implementación del trait [`ToastTrigger`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FStoreActivityRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A15%7D%7D%5D%2C%2245a8fa6a-2395-4304-80d4-e0331a9790a7%22%5D "Go to definition").
- **Captura de pantalla 5:** Inclusión del trait en la clase de solicitud.

Estas capturas de pantalla ayudarán a visualizar cada paso del proceso.