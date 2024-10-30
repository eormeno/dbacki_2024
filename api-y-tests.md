## Tutorial: Creación de Controladores y Rutas para Implementar una API en Laravel con Blade y Tailwind CSS

### Introducción

En este tutorial, aprenderás a crear controladores y rutas para implementar una API en Laravel. También aprenderás a escribir tests para tus endpoints. Utilizaremos Blade y Tailwind CSS para la parte visual. 

### ¿Qué es una API?

Una API (Application Programming Interface) es un conjunto de reglas que permite que diferentes aplicaciones se comuniquen entre sí. En el contexto de una aplicación web, una API permite que el frontend y el backend se comuniquen.

### ¿Qué es un Endpoint?

Un endpoint es una URL específica en una API que realiza una acción particular. Por ejemplo, un endpoint puede devolver una lista de usuarios o crear un nuevo usuario.

### Creación del Controlador

Primero, crearemos un controlador para manejar las solicitudes de nuestra API. Utilizaremos el comando `php artisan make:controller` para crear el controlador.

```bash
php artisan make:controller PatientActivitiesApiController
```

Este comando creará un archivo llamado `PatientActivitiesApiController.php` en el directorio `app/Http/Controllers`.

### Código del Controlador

A continuación, añadiremos el código para manejar las solicitudes en nuestro controlador.

```php
<?php

namespace App\Http\Controllers;

use App\Models\Patient;
use App\Models\PatientActivity;
use Illuminate\Http\Request;

class PatientActivitiesApiController extends Controller
{
    public function getPatientActivities(string $code)
    {
        $patient = Patient::where('codigo', $code)->first();
        if (!$patient) {
            return response()->json(['message' => 'Patient not found'], 404);
        }
        $activities = PatientActivity::where('patient_id', $patient->id)->with('activity')->get();
        $activities = $this->hidePatientActivitesResponseFields($activities);
        $patient = $this->hidePatientResponseFields($patient);
        $response = [
            'patient' => $patient,
            'activities' => $activities
        ];
        return response()->json($response);
    }

    private function hidePatientResponseFields($patient)
    {
        return [
            'nombres' => $patient->nombres,
            'apellidos' => $patient->apellidos,
            'nacimiento' => $patient->nacimiento,
            'sexo' => $patient->sexo,
            'telefono' => $patient->telefono,
        ];
    }

    private function hidePatientActivitesResponseFields($activities)
    {
        return $activities->map(function ($activity) {
            return [
                'activity_id' => $activity->activity_id,
                'activity_name' => $activity->activity->name,
                'activity_description' => $activity->activity->description,
                'activity_thumbnail' => $activity->activity->thumbnail,
                'description' => $activity->description,
                'reasons' => $activity->reasons,
                'goals' => $activity->goals,
                'indicators' => $activity->indicators,
            ];
        });
    }
}
```

**Explicación del Código del Controlador:**

- `getPatientActivities`: Este método recibe un código de paciente, busca al paciente en la base de datos y, si lo encuentra, obtiene sus actividades. Luego, devuelve una respuesta JSON con los datos del paciente y sus actividades.
- `hidePatientResponseFields`: Este método oculta ciertos campos del paciente en la respuesta.
- `hidePatientActivitesResponseFields`: Este método oculta ciertos campos de las actividades del paciente en la respuesta.

### Creación de Rutas

Ahora, crearemos las rutas para nuestra API. Añadiremos las rutas en el archivo `routes/api.php`.

```php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\PatientActivitiesApiController;

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');

Route::get('/patient-activities/{code}', [PatientActivitiesApiController::class, 'getPatientActivities']);
```

**Explicación del Código de Rutas:**

- `Route::get('/patient-activities/{code}', [PatientActivitiesApiController::class, 'getPatientActivities'])`: Esta ruta define un endpoint GET que llama al método `getPatientActivities` del controlador `PatientActivitiesApiController`.

### Creación de Tests

Es importante escribir tests para asegurarnos de que nuestra API funciona correctamente. Crearemos un test para nuestro endpoint en el archivo `tests/Feature/PatientActivityEndPointsTest.php`.

```php
<?php

use Tests\TestHelpers;
use App\Models\Patient;
use App\Models\Activity;
use App\Models\PatientActivity;

test('El EndPoint patient-activities/{codigo} retorna un json válido', function () {
    $user = TestHelpers::rootUser();
    $patient = Patient::factory()->create();
    $activity = Activity::factory()->create();
    PatientActivity::factory()->create([
        'user_id' => $user->id,
        'patient_id' => $patient->id,
        'activity_id' => $activity->id,
    ]);

    $response = $this->get("/api/patient-activities/$patient->codigo");

    $response->assertStatus(200);
    $response->assertJsonStructure([
        'patient' => [
            'nombres',
            'apellidos',
            'nacimiento',
            'sexo',
            'telefono',
        ],
        'activities' => [
            '*' => [
                'activity_id',
                'activity_name',
                'activity_description',
                'activity_thumbnail',
                'description',
                'reasons',
                'goals',
                'indicators',
            ],
        ],
    ]);
});
```

**Explicación del Código de Tests:**

- `TestHelpers::rootUser()`: Crea un usuario con rol de root.
- `Patient::factory()->create()`: Crea un paciente usando una fábrica.
- `Activity::factory()->create()`: Crea una actividad usando una fábrica.
- `PatientActivity::factory()->create()`: Crea una actividad de paciente asociada al usuario, paciente y actividad creados.
- `assertStatus(200)`: Verifica que la respuesta tenga un código de estado 200.
- `assertJsonStructure`: Verifica que la estructura del JSON de la respuesta coincida con la estructura esperada.

### Clase TestHelpers

La clase `TestHelpers` se creó para proporcionar métodos auxiliares que faciliten la creación de usuarios y roles durante las pruebas.

```php
<?php

namespace Tests;

use App\Models\User;
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;
use Spatie\Permission\PermissionRegistrar;

class TestHelpers
{
    private const SEE_PANEL = 'see-panel';

    public static function rootUser()
    {
        Role::create(['name' => 'root']);
        $user = User::factory()->rootUser()->create()->assignRole('root');
        return $user;
    }

    public static function createRegisteredRole()
    {
        Permission::create(['name' => self::SEE_PANEL]);
        $registered_role = Role::create(['name' => 'registered']);
        $registered_role->givePermissionTo(self::SEE_PANEL);
        return $registered_role;
    }

    public static function registeredUser()
    {
        self::createRegisteredRole();
        $user = User::factory()->create()->assignRole('registered');
        return $user;
    }

    public static function rootDefaultPassword()
    {
        return env('ADMIN_PASSWORD');
    }

    public static function fakeUsersPassword()
    {
        return 'password';
    }
}
```

**Explicación de la Clase TestHelpers:**

- `rootUser`: Crea un rol de root y un usuario con ese rol.
- `createRegisteredRole`: Crea un rol de usuario registrado y le asigna un permiso.
- `registeredUser`: Crea un usuario con el rol de registrado.
- `rootDefaultPassword`: Devuelve la contraseña por defecto del usuario root.
- `fakeUsersPassword`: Devuelve una contraseña falsa para los usuarios de prueba.

### Conclusión

En este tutorial, aprendiste a crear un controlador y rutas para una API en Laravel. También aprendiste a escribir tests para tus endpoints y a utilizar la clase `TestHelpers` para facilitar la creación de usuarios y roles durante las pruebas. Utiliza estos conceptos y ejemplos para crear tus propias APIs y tests en Laravel.