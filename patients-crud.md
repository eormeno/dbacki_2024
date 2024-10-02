# Tutorial: Patients CRUD
## Contenido
1. [Objetivo](#objetivo)
2. [Introducción](#introducción)
3. [Componentes necesarios](#componentes-necesarios)
4. [Migración](#migración)
5. [Factory](#factory)
6. [Seeder](#seeder)
7. [Controlador](#controlador)
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


