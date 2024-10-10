### Explicación de FormRequest en Laravel

En Laravel, los [`FormRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A31%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition") son una forma conveniente de manejar la validación de datos de entrada de formularios. Estos se utilizan para encapsular la lógica de validación y autorización en una clase separada, lo que hace que el código del controlador sea más limpio y manejable.

#### Métodos principales de FormRequest

1. **authorize()**: Este método se utiliza para determinar si el usuario está autorizado para realizar la solicitud. Devuelve un valor booleano ([`true`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A13%2C%22character%22%3A15%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition") o `false`). En el ejemplo de [`PatientRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A6%2C%22character%22%3A6%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), siempre devuelve [`true`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A13%2C%22character%22%3A15%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), lo que significa que cualquier usuario está autorizado para hacer esta solicitud.

2. **rules()**: Este método devuelve un array de reglas de validación que se aplican a los datos de la solicitud. En el ejemplo de [`PatientRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A6%2C%22character%22%3A6%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), se definen varias reglas para validar los campos como [`codigo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A24%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), [`apellidos`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A25%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), [`nombres`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A26%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), [`dni`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A27%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), [`nacimiento`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A28%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), [`sexo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A29%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), [`telefono`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A30%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), [`email`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A31%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition") y [`direccion`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A32%2C%22character%22%3A13%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition").

3. **messages()**: Este método opcional se utiliza para personalizar los mensajes de error de validación. Si no se define, Laravel usará los mensajes de error predeterminados.

### Ejemplo de 

PatientRequest.php



```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class PatientRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     */
    public function authorize(): bool
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, \Illuminate\Contracts\Validation\ValidationRule|array<mixed>|string>
     */
    public function rules(): array
    {
        return [
            'codigo' => 'required|unique:patients',
            'apellidos' => 'required',
            'nombres' => 'required',
            'dni' => 'required|unique:patients',
            'nacimiento' => 'required|date',
            'sexo' => 'required',
            'telefono' => 'required',
            'email' => 'required|email|unique:patients',
            'direccion' => 'required',
        ];
    }

    /**
     * Get custom messages for validator errors.
     *
     * @return array
     */
    public function messages(): array
    {
        return [
            'codigo.required' => 'El código es obligatorio.',
            'codigo.unique' => 'El código ya está en uso.',
            'dni.required' => 'El DNI es obligatorio.',
            'dni.unique' => 'El DNI ya está en uso.',
            'email.required' => 'El correo electrónico es obligatorio.',
            'email.email' => 'El correo electrónico debe ser una dirección válida.',
            'email.unique' => 'El correo electrónico ya está en uso.',
            // Otros mensajes personalizados...
        ];
    }
}
```

### Uso en el método store() de PatientController.php

El método `store()` en el controlador `PatientController` utiliza el [`PatientRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A6%2C%22character%22%3A6%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition") para validar los datos de entrada antes de crear un nuevo registro de paciente. Aquí está el código del método `store()`:

```php
public function store(PatientRequest $request)
{
    // Los datos ya están validados en este punto
    $validatedData = $request->validated();

    // Crear un nuevo paciente con los datos validados
    $patient = new Patient($validatedData);
    $patient->save();

    // Redirigir o devolver una respuesta
    return redirect()->route('patients.index')->with('success', 'Paciente creado exitosamente.');
}
```

### Explicación del método store()

1. **Inyección de Dependencias**: El método `store()` recibe una instancia de [`PatientRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A6%2C%22character%22%3A6%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition"), lo que significa que Laravel automáticamente validará la solicitud antes de que el método sea ejecutado.
2. **Validación**: Los datos de la solicitud ya están validados en este punto, por lo que se pueden obtener directamente usando `$request->validated()`.
3. **Creación del Paciente**: Se crea una nueva instancia de `Patient` con los datos validados y se guarda en la base de datos.
4. **Redirección**: Finalmente, se redirige al usuario a la lista de pacientes con un mensaje de éxito.

Este enfoque hace que el código del controlador sea más limpio y enfocado en la lógica de negocio, delegando la validación y autorización a la clase [`FormRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FC%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FRequests%2FPatientRequest.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A31%7D%7D%5D%2C%224d792e2b-54bf-407a-aafb-070af0806ee3%22%5D "Go to definition").