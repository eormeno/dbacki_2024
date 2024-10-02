# Tutorial: Patients CRUD
## Contenido
1. [Objetivo](#objetivo)
2. [Introducción](#introducción)
3. [Componentes necesarios](#componentes-necesarios)
4. [Migración](#migración)
5. [Factory](#factory)
6. [Seeder](#seeder)
7. [Controlador](#controlador)
8. [Las vistas](#las-vistas)
9. [Rutas](#rutas)
## Objetivo
Crear un manager para gestionar los pacientes de la aplicación.
## Introducción
De acuerdo a lo epecificado por el cliente, los usuarios con rol de médico o terapeuta deben poder gestionar los pacientes de la aplicación. Para ello, vamos a crear un manager que permita realizar operaciones CRUD (Create, Read, Update, Delete) de los pacientes.
## Componentes necesarios
Ejecutamos el siguiente comando artisan para crear todos los componentes necesarios:
```bash
php artisan make:model Patient -cmfrs
```
Este comando creará:
1. El modelo `Patient` en `app/Models/Patient.php`
2. Un controlador `PatientController` en `app/Http/Controllers/PatientController.php`
3. Una migración `create_patients_table` en `database/migrations`
4. El factory `PatientFactory` en `database/factories/PatientFactory.php`
5. El seeder `PatientSeeder` en `database/seeders/PatientSeeder.php`

## Migración
Modifica el método `up()` del archivo `database/migrations/yyyy-mm-dd-nnnnn-create_patients_table.php` para definir la estructura de la tabla para los pacientes:
```php
    ...
    public function up(): void
    {
        Schema::create('patients', function (Blueprint $table) {
            $table->id();
            $table->string('codigo')->unique();
            $table->string('apellidos');
            $table->string('nombres');
            $table->string('dni')->unique();
            $table->date('nacimiento');
            $table->string('sexo');
            $table->string('telefono');
            $table->string('email')->unique();
            $table->string('direccion');
            $table->text('observaciones')->nullable();
            $table->timestamps();
        });
    }
    ...
```

## Factory
Modifica el archivo `database/factories/PatientFactory.php` para definir los datos de prueba para los pacientes:
```php
    ...
    public function definition(): array {
        $fake_first = fake()->firstName();
        $fake_last = fake()->lastName();
        $fake_email = FakeUtils::email($fake_first, $fake_last);
        return [
            'codigo' => $this->faker->unique()->regexify('[A-Z]{3}[0-9]{3}'),
            'apellidos' => $fake_last,
            'nombres' => $fake_first,
            'dni' => $this->faker->unique()->numerify('########'),
            'nacimiento' => $this->faker->date(),
            'sexo' => $this->faker->randomElement(['M', 'F']),
            'telefono' => $this->faker->phoneNumber(),
            'email' => $fake_email,
            'direccion' => $this->faker->address(),
            'observaciones' => $this->faker->optional()->sentence(),
        ];
    }
    ...
```

## Seeder
Modifica el archivo `database/seeders/PatientSeeder.php` para definir la cantidad de pacientes de prueba:
```php
    ...
    public function run(): void {
        Patient::factory(10)->create();
    }
    ...
```
También debes modificar el archivo `database/seeders/DatabaseSeeder.php` para incluir el seeder de pacientes:
```php
    class DatabaseSeeder extends Seeder {
        public function run(): void {
            fake()->seed(10);   // Esto es para generar siempre los mismos datos
            $this->call(PermissionsSeeder::class);
            $this->call(UsersSeeder::class);
            $this->call(PatientSeeder::class); // Agregar esta línea
        }
    }
```

## Controlador
Modifica el archivo `app/Http/Controllers/PatientController.php` para definir los métodos necesarios para realizar las operaciones CRUD de los pacientes:
```php
    ...
    public function index(): View {
        $patients = Patient::all();
        return view('patients.index', compact('patients'));
    }

    public function create(): View {
        return view('patients.create');
    }

    public function store(Request $request): RedirectResponse {
        $request->validate([
            'codigo' => 'required|unique:patients',
            'apellidos' => 'required',
            'nombres' => 'required',
            'dni' => 'required|unique:patients',
            'nacimiento' => 'required|date',
            'sexo' => 'required',
            'telefono' => 'required',
            'email' => 'required|email|unique:patients',
            'direccion' => 'required',
        ]);
        Patient::create($request->all());
        return redirect()->route('patients.index');
    }

    public function show(Patient $patient): View {
        return view('patients.show', compact('patient'));
    }

    public function edit(Patient $patient): View {
        return view('patients.edit', compact('patient'));
    }

    public function update(Request $request, Patient $patient): RedirectResponse {
        $request->validate([
            'codigo' => 'required|unique:patients,codigo,'.$patient->id,
            'apellidos' => 'required',
            'nombres' => 'required',
            'dni' => 'required|unique:patients,dni,'.$patient->id,
            'nacimiento' => 'required|date',
            'sexo' => 'required',
            'telefono' => 'required',
            'email' => 'required|email|unique:patients,email,'.$patient->id,
            'direccion' => 'required',
        ]);
        $patient->update($request->all());
        return redirect()->route('patients.index');
    }

    public function destroy(Patient $patient): RedirectResponse {
        $patient->delete();
        return redirect()->route('patients.index');
    }
    ...
```
## Las vistas
Crea las vistas necesarias en el directorio `resources/views/patients`:

1. `index.blade.php`
```html
<x-event-layout>
	<x-slot name="title">Pacientes</x-slot>
    <a href="{{ route('patients.create') }}">Nuevo paciente</a>
    <table>
        <thead>
            <tr>
                <th>Código</th>
                <th>Apellidos</th>
                <th>Nombres</th>
                <th>DNI</th>
                <th>Fecha de nacimiento</th>
                <th>Sexo</th>
                <th>Teléfono</th>
                <th>Email</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($patients as $patient)
                <tr>
                    <td>{{ $patient->codigo }}</td>
                    <td>{{ $patient->apellidos }}</td>
                    <td>{{ $patient->nombres }}</td>
                    <td>{{ $patient->dni }}</td>
                    <td>{{ $patient->nacimiento }}</td>
                    <td>{{ $patient->sexo }}</td>
                    <td>{{ $patient->telefono }}</td>
                    <td>{{ $patient->email }}</td>
                    <td>
                        <a href="{{ route('patients.show', $patient) }}">Ver</a>
                        <a href="{{ route('patients.edit', $patient) }}">Editar</a>
                        <form action="{{ route('patients.destroy', $patient) }}" method="POST">
                            @csrf
                            @method('DELETE')
                            <button type="submit">Eliminar</button>
                        </form>
                    </td>
                </tr>
            @endforeach
        </tbody>
    </table>
</x-event-layout>
```

2. `create.blade.php`
```html
<x-event-layout>
	<x-slot name="title">Nuevo Pacientes</x-slot>
    <form action="{{ route('patients.store') }}" method="POST">
        @csrf
        <label for="codigo">Código</label>
        <input type="text" name="codigo" required>
        <label for="apellidos">Apellidos</label>
        <input type="text" name="apellidos" required>
        <label for="nombres">Nombres</label>
        <input type="text" name="nombres" required>
        <label for="dni">DNI</label>
        <input type="text" name="dni" required>
        <label for="nacimiento">Fecha de nacimiento</label>
        <input type="date" name="nacimiento" required>
        <label for="sexo">Sexo</label>
        <select name="sexo" required>
            <option value="M">Masculino</option>
            <option value="F">Femenino</option>
        </select>
        <label for="telefono">Teléfono</label>
        <input type="text" name="telefono" required>
        <label for="email">Email</label>
        <input type="email" name="email" required>
        <label for="direccion">Dirección</label>
        <input type="text" name="direccion" required>
        <label for="observaciones">Observaciones</label>
        <textarea name="observaciones"></textarea>
        <button type="submit">Guardar</button>
    </form>
</x-event-layout>
```

3. `show.blade.php`
```html
<x-event-layout>
	<x-slot name="title">Detalle del paciente</x-slot>
    <p>Código: {{ $patient->codigo }}</p>
    <p>Apellidos: {{ $patient->apellidos }}</p>
    <p>Nombres: {{ $patient->nombres }}</p>
    <p>DNI: {{ $patient->dni }}</p>
    <p>Fecha de nacimiento: {{ $patient->nacimiento }}</p>
    <p>Sexo: {{ $patient->sexo }}</p>
    <p>Teléfono: {{ $patient->telefono }}</p>
    <p>Email: {{ $patient->email }}</p>
    <p>Dirección: {{ $patient->direccion }}</p>
    <p>Observaciones: {{ $patient->observaciones }}</p>
    <a href="{{ route('patients.index') }}">Volver</a>
</x-event-layout>
```

4. `edit.blade.php`
```html
<x-event-layout>
	<x-slot name="title">Editar paciente</x-slot>
    <form action="{{ route('patients.update', $patient) }}" method="POST">
        @csrf
        @method('PUT')
        <label for="codigo">Código</label>
        <input type="text" name="codigo" value="{{ $patient->codigo }}" required>
        <label for="apellidos">Apellidos</label>
        <input type="text" name="apellidos" value="{{ $patient->apellidos }}" required>
        <label for="nombres">Nombres</label>
        <input type="text" name="nombres" value="{{ $patient->nombres }}" required>
        <label for="dni">DNI</label>
        <input type="text" name="dni" value="{{ $patient->dni }}" required>
        <label for="nacimiento">Fecha de nacimiento</label>
        <input type="date" name="nacimiento" value="{{ $patient->nacimiento }}" required>
        <label for="sexo">Sexo</label>
        <select name="sexo" required>
            <option value="M" @if ($patient->sexo == 'M') selected @endif>Masculino</option>
            <option value="F" @if ($patient->sexo == 'F') selected @endif>Femenino</option>
        </select>
        <label for="telefono">Teléfono</label>
        <input type="text" name="telefono" value="{{ $patient->telefono }}" required>
        <label for="email">Email</label>
        <input type="email" name="email" value="{{ $patient->email }}" required>
        <label for="direccion">Dirección</label>
        <input type="text" name="direccion" value="{{ $patient->direccion }}" required>
        <label for="observaciones">Observaciones</label>
        <textarea name="observaciones">{{ $patient->observaciones }}</textarea>
        <button type="submit">Guardar</button>
    </form>
</x-event-layout>
```

## Rutas
Modifica el archivo `routes/web.php` para definir las rutas necesarias para el controlador de pacientes:
```php
    ...
    Route::resource('patients', PatientController::class)->middleware('auth');
    ...
```
> **Nota:** El middleware `auth` se encarga de verificar que el usuario esté autenticado antes de permitir el acceso a las rutas del controlador de pacientes.

Ejecuta el comando `php artisan route:list` para verificar que las rutas se han creado correctamente, lo cual desplegará por consola una tabla similar a la siguiente:

| Method	| URI						| Name				| Action					|
|-----------|---------------------------|-------------------|---------------------------|
| GET		| `patients`				| patients.index	| PatientController@index	|
| POST		| `patients`				| patients.store	| PatientController@store	|
| GET		| `patients/create`			| patients.create	| PatientController@create	|
| GET		| `patients/{patient}`		| patients.show		| PatientController@show	|
| PUT PATCH	| `patients/{patient}`		| patients.update	| PatientController@update	|
| DELETE	| `patients/{patient}`		| patients.destroy	| PatientController@destroy	|
| GET		| `patients/{patient}/edit`	| patients.edit		| PatientController@edit	| 

El método `Route::resource()` crea las rutas necesarias para realizar las operaciones CRUD de un recurso. En este caso, el recurso es el controlador `PatientController`. Si deseas crear las rutas manualmente, debes hacerlo de la siguiente manera:
```php
Route::get('patients', [PatientController::class, 'index'])->name('patients.index');
Route::post('patients', [PatientController::class, 'store'])->name('patients.store');
Route::get('patients/create', [PatientController::class, 'create'])->name('patients.create');
Route::get('patients/{patient}', [PatientController::class, 'show'])->name('patients.show');
Route::put('patients/{patient}', [PatientController::class, 'update'])->name('patients.update');
Route::delete('patients/{patient}', [PatientController::class, 'destroy'])->name('patients.destroy');
Route::get('patients/{patient}/edit', [PatientController::class, 'edit'])->name('patients.edit');
```
