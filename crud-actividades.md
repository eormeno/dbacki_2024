# Tutorial de Laravel Blade y Tailwind CSS: CRUD de Actividades

## Contexto
En este tutorial, aprenderás a programar un CRUD (Crear, Leer, Actualizar, Eliminar) de actividades utilizando Laravel, Blade y Tailwind CSS. Este tutorial está diseñado para principiantes y se basa en los códigos fuentes del proyecto.

## Comandos Artisan Utilizados

### Crear Modelo, Controlador, Factory, Seeder, Form Requests y Policy
```bash
php artisan make:model Activity -a
```
Este comando crea las clases necesarias para el modelo [`Activity`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fcrud-diff-prompt.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A51%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A15%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition"), incluyendo el controlador, factory, seeder, form requests y policy.

### Crear Vistas Vacías del CRUD
```bash
php artisan make:view activities.create
php artisan make:view activities.show
php artisan make:view activities.index
php artisan make:view activities.edit
```
Estos comandos crean las vistas vacías para las operaciones CRUD.

## Instalación del Paquete Intervention de Laravel
Para manejar imágenes en Laravel, instalaremos el paquete `intervention/image-laravel`:
```bash
composer require intervention/image-laravel
```
Este paquete es necesario para manejar imágenes, que se almacenarán en formato base64 en la tabla de actividades. Asegúrate de habilitar la extensión [`gd`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fcrud-diff-prompt.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A18%2C%22character%22%3A255%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition") en el archivo `php.ini`:
```ini
extension=gd
```

## Código del Proyecto

### 1. Migración de la Tabla de Actividades
```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('activities', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('description')->nullable();
            $table->text('image')->nullable();
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('activities');
    }
};
```
Este código crea la tabla [`activities`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fcrud-diff-prompt.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A9%2C%22character%22%3A27%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A19%2C%22character%22%3A9%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fdatabase%2Fmigrations%2F2024_10_14_120318_create_activities_table.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A13%2C%22character%22%3A24%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fresources%2Fviews%2Factivities%2Fshow.blade.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A2%2C%22character%22%3A23%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition") con los campos [`name`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A2%2C%22character%22%3A0%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fresources%2Fviews%2Factivities%2Fshow.blade.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A1%2C%22character%22%3A12%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fdatabase%2Fmigrations%2F2024_10_14_120318_create_activities_table.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A15%2C%22character%22%3A28%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fresources%2Fviews%2Fpatients%2Fcreate.blade.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A53%2C%22character%22%3A73%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fresources%2Fviews%2Flayouts%2Fapp.blade.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A5%2C%22character%22%3A10%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition"), [`description`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fresources%2Fviews%2Factivities%2Fshow.blade.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A13%2C%22character%22%3A34%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fdatabase%2Fmigrations%2F2024_10_14_120318_create_activities_table.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A16%2C%22character%22%3A26%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition"), [`image`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fcrud-diff-prompt.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A16%2C%22character%22%3A30%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fdatabase%2Fmigrations%2F2024_10_14_120318_create_activities_table.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A17%2C%22character%22%3A26%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A36%2C%22character%22%3A32%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fresources%2Fviews%2Factivities%2Fshow.blade.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A14%2C%22character%22%3A19%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition") y las marcas de tiempo.

### 2. Modelo de Actividades: [`Activity`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fcrud-diff-prompt.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A51%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A15%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition")
```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Activity extends Model
{
    use HasFactory;

    protected $fillable = ['name', 'description', 'image'];
}
```
El modelo [`Activity`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fcrud-diff-prompt.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A51%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A15%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition") define los campos que se pueden llenar masivamente.

### 3. Controlador de Actividades: [`ActivityController`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A10%2C%22character%22%3A6%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition")
```php
namespace App\Http\Controllers;

use App\Models\Activity;
use App\Http\Requests\StoreActivityRequest;
use App\Http\Requests\UpdateActivityRequest;
use App\Traits\DebugHelper;
use Intervention\Image\Laravel\Facades\Image;

class ActivityController extends Controller
{
    use DebugHelper;

    public function index()
    {
        $activities = Activity::latest()->paginate(5);
        return view('activities.index', compact('activities'));
    }

    public function create()
    {
        return view('activities.create');
    }

    public function store(StoreActivityRequest $request)
    {
        $file = $request->file('image');
        $contents = file_get_contents($file);
        $base64Image = base64_encode($contents);
        $validated = $request->validated();
        $validated['image'] = $base64Image;
        Activity::create($validated);
        return redirect()->route('activities.index');
    }

    public function show(Activity $activity)
    {
        return view('activities.show', compact('activity'));
    }

    public function edit(Activity $activity)
    {
        return view('activities.edit', compact('activity'));
    }

    public function update(UpdateActivityRequest $request, Activity $activity)
    {
        $activity->update($request->validated());
        return redirect()->route('activities.index');
    }

    public function destroy(Activity $activity)
    {
        $activity->delete();
        return redirect()->route('activities.index');
    }
}
```
El método `store()` guarda la imagen en la base de datos en formato base64.

### 4. Seeder de Actividades: `ActivitySeeder`
```php
<?php
namespace Database\Seeders;

use App\Models\Activity;
use Illuminate\Database\Seeder;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;

class ActivitySeeder extends Seeder {
    use WithoutModelEvents;

    public function run(): void
    {
        Activity::factory()->count(3)->create();
    }
}
```
Este seeder crea 10 actividades utilizando la fábrica de actividades.

### 5. Factory de Actividades: `ActivityFactory`
```php
<?php
namespace Database\Factories;

use Illuminate\Support\Facades\Http;
use Intervention\Image\Laravel\Facades\Image;
use Illuminate\Database\Eloquent\Factories\Factory;

class ActivityFactory extends Factory {
    private function downloadImage(int $width, int $height) : ?string {
        $response = Http::get("https://picsum.photos/{$width}/{$height}");
        if ($response->successful()) {
            return base64_encode($response->body());
        }
        return null;
    }

    public function definition() {
        $base64Image = $this->downloadImage(255, 255);
        if ($base64Image === null) {
            $image = Image::canvas(255, 255, '#ccc');
            $base64Image = $image->encode('data-url');
        }

        return [
            'name' => $this->faker->sentence(2),
            'description' => $this->faker->paragraph(),
            'image' => $base64Image
        ];
    }
}
```
La fábrica de actividades utiliza el método `downloadImage()` para descargar imágenes de internet y convertirlas a base64. Si no se puede descargar la imagen, se crea una imagen de relleno.

### 6. Vista de Creación de Actividades: 

create.blade.php

```php
<x-crud-layout>
    <x-slot name="title">Nueva Actividad</x-slot>

    <a href="{{ route('activities.index') }}">
        <div
            class="inline-flex items-center px-4 py-2 mb-2 bg-indigo-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-indigo-700 active:bg-indigo-900 focus:outline-none focus:border-indigo-900 focus:ring ring-indigo-300 disabled:opacity-25 transition ease-in-out duration-150">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M10.5 19.5 3 12m0 0 7.5-7.5M3 12h18" />
            </svg>
        </div>
    </a>

    <form action="{{ route('activities.store') }}" method="POST" class="mt-2" novalidate
        enctype="multipart/form-data">
        @csrf
        <div class="mt-2">
            <x-label for="name" value="Nombre" />
            <x-input id="name" class="block mt-1 w-full" type="text" name="name" :value="old('name')" required
                autofocus autocomplete="name" />
            <x-input-error for="name" class="mt-2" />
        </div>
        <div class="mt-2">
            <x-label for="description" value="Descripción" />
            <textarea name="description" id="description" cols="30" rows="5"
                class="border-gray-300 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-300 focus:border-indigo-500 dark:focus:border-indigo-600 focus:ring-indigo-500 dark:focus:ring-indigo-600 rounded-md shadow-sm block mt-1 w-full"
                required>{{ old('description') }}</textarea>
        </div>
        <div class="mb-3">
            <x-label for="image" value="Imagen" />
            <x-input id="image" class="block mt-1 w-full" type="file" name="image" :value="old('image')" required
                autocomplete="image" />
            <x-input-error for="image" class="mt-2" />
        </div>
        <div class="flex items-center justify-end mt-4">
            <x-button class="ms-4">Crear actividad</x-button>
        </div>
    </form>

</x-crud-layout>
```
Esta vista permite crear una nueva actividad.

### 7. Vista de Edición de Actividades: `activities/edit.blade.php`
```php
<x-crud-layout>
    <x-slot name="title">Modificar actividad</x-slot>

    <a href="{{ route('activities.index') }}">
        <div
            class="inline-flex items-center px-4 py-2 mb-2 bg-indigo-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-indigo-700 active:bg-indigo-900 focus:outline-none focus:border-indigo-900 focus:ring ring-indigo-300 disabled:opacity-25 transition ease-in-out duration-150">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M10.5 19.5 3 12m0 0 7.5-7.5M3 12h18" />
            </svg>
        </div>
    </a>

    <form action="{{ route('activities.update', $activity) }}" method="POST" class="mt-2"
        enctype="multipart/form-data" novalidate>
        @csrf
        <div class="mt-2">
            <x-label for="name" value="Nombre" />
            <x-input id="name" class="block mt-1 w-full" type="text" name="name"
                value="{{ $activity->name }}" required autofocus autocomplete="name" />
            <x-input-error for="name" class="mt-2" />
        </div>
        <div class="mt-2">
            <x-label for="description" value="Descripción" />
            <textarea name="description" id="description" cols="30" rows="5"
                class="border-gray-300 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-300 focus:border-indigo-500 dark:focus:border-indigo-600 focus:ring-indigo-500 dark:focus:ring-indigo-600 rounded-md shadow-sm block mt-1 w-full"
                required>{{ $activity->description }}</textarea>
        </div>
        <div class="mt-2">
            <img src="data:image/png;base64,{{ $activity->image }}" alt="Imagen de la actividad" width="255">
        </div>
        <div class="mt-2">
            <x-label for="image" value="Imagen" />
            <x-input id="image" class="block mt-1 w-full" type="file" name="image" :value="old('image')" required
                autocomplete="image" />
            <x-input-error for="image" class="mt-2" />
        </div>
        <div class="flex items-center justify-end mt-4">
            <x-button class="ml-4">
                {{ __('Modificar actividad') }}
            </x-button>
        </div>
    </form>
</x-crud-layout>
```
Esta vista permite editar una actividad existente y muestra la imagen almacenada en formato base64.

### 8. Vista de Listado de Actividades: `activities/index.blade.php`
```php
<x-crud-layout>
    <x-slot name="title">Actividades</x-slot>

    <a href="{{ route('activities.create') }}"
        class="inline-flex items-center px-4 py-2 mb-2 bg-indigo-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-indigo-700 active:bg-indigo-900 focus:outline-none focus:border-indigo-900 focus:ring ring-indigo-300 disabled:opacity-25 transition ease-in-out duration-150">
        Nueva actividad
    </a>
    <div class="overflow-x-hidden">
        <table class="min-w-full divide-y divide-gray-200">
            <thead class="bg-gray-50">
                <tr>
                    <th scope="col"
                        class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                        Nombre</th>
                    <th scope="col"
                        class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                        Descripción</th>
                    <th scope="col"
                        class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                        Acciones</th>
                </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
                @foreach ($activities as $key => $activity)
                    <tr class="{{ $key % 2 === 0 ? 'bg-gray-100' : 'bg-white' }} hover:bg-gray-200">
                        <td class="px-4 py-2 whitespace-nowrap text-sm font-medium text-gray-900">
                            {{ $activity->name }}</td>
                        <td class="px-4 py-2 whitespace-nowrap text-sm font-medium text-gray-900">
                            {{ Str::limit($activity->description, 30) }}</td>
                        <td class="px-4 py-2 whitespace-nowrap text-sm text-gray-500">
                            <div class="flex items-center space-x-2">
                                <a href="{{ route('activities.show', $activity) }}"
                                    class="text-indigo-600 hover:text-indigo-900">
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                                        stroke-width="1.5" stroke="currentColor" class="size-6">
                                        <path stroke-linecap="round" stroke-linejoin="round"
                                            d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" />
                                        <path stroke-linecap="round" stroke-linejoin="round"
                                            d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
                                    </svg>
                                </a>
                                <a href="{{ route('activities.edit', $activity) }}"
                                    class="text-indigo-600 hover:text-indigo-900">
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                                        stroke-width="1.5" stroke="currentColor" class="size-6">
                                        <path stroke-linecap="round" stroke-linejoin="round"
                                            d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                                    </svg>
                                </a>
                                <form action="{{ route('activities.destroy', $activity) }}" method="POST">
                                    @csrf
                                    @method('DELETE')
                                    <button type="submit">
                                        <div class="text-red-700 hover:text-red-900">
                                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                                                stroke-width="1.5" stroke="currentColor" class="size-6">
                                                <path stroke-linecap="round" stroke-linejoin="round"
                                                    d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
                                            </svg>
                                        </div>
                                    </button>
                                </form>
                            </div>
                        </td>
                    </tr>
                @endforeach
            </tbody>
        </table>
    </div>

    <x-slot name="links">
        {{ $activities->links() }}
    </x-slot>
</x-crud-layout>
```
Esta vista muestra una lista de actividades con opciones para ver, editar y eliminar cada actividad.

### 9. Vista de Detalle de Actividades: 

show.blade.php

```php
<x-crud-layout>
    <x-slot name="title">Detalle de la actividad</x-slot>
    <a href="{{ route('activities.index') }}">
        <div
            class="inline-flex items-center px-4 py-2 mb-2 bg-indigo-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-indigo-700 active:bg-indigo-900 focus:outline-none focus:border-indigo-900 focus:ring ring-indigo-300 disabled:opacity-25 transition ease-in-out duration-150">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M10.5 19.5 3 12m0 0 7.5-7.5M3 12h18" />
            </svg>
        </div>
    </a>

    <p>Nombre: {{ $activity->name }}</p>
    <p>Descripción: {{ $activity->description }}</p>
    <img src="data:image/png;base64,{{ $activity->image }}" alt="Imagen de la actividad" width="255">

</x-crud-layout>
```
Esta vista muestra los detalles de una actividad, incluyendo la imagen almacenada en formato base64.

### 10. Política de Actividades: `ActivityPolicy`
```php
namespace App\Policies;

use App\Models\Activity;
use App\Models\User;
use Illuminate\Auth\Access\HandlesAuthorization;

class ActivityPolicy
{
    use HandlesAuthorization;

    public function viewAny(User $user)
    {
        return true;
    }

    public function view(User $user, Activity $activity)
    {
        return true;
    }

    public function create(User $user)
    {
        return true;
    }

    public function update(User $user, Activity $activity)
    {
        return true;
    }

    public function delete(User $user, Activity $activity)
    {
        return true;
    }
}
```
Esta clase define las políticas de autorización para las operaciones CRUD en el modelo [`Activity`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fcrud-diff-prompt.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A7%2C%22character%22%3A51%7D%7D%2C%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A4%2C%22character%22%3A15%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition").

### 11. Clase [`StoreActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A5%2C%22character%22%3A22%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition")
```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreActivityRequest extends FormRequest {
    public function authorize(): bool {
        return true;
    }

    public function rules(): array {
        return [
            'name' => ['required', 'string', 'max:255'],
            'description' => ['required', 'string'],
            'image' => ['required', 'image','mimes:jpeg,png,jpg,gif,svg', 'max:2048'],
        ];
    }

    public function messages(): array {
        return [
            'name.required' => 'El nombre es requerido',
            'name.string' => 'El nombre debe ser una cadena de texto',
            'name.max' => 'El nombre no debe exceder los 255 caracteres',
            'description.required' => 'La descripción es requerida',
            'description.string' => 'La descripción debe ser una cadena de texto',
            'image.required' => 'La imagen es requerida',
            'image.image' => 'La imagen debe ser un archivo de imagen',
        ];
    }
}
```
Esta clase valida las solicitudes de creación de actividades.

### 12. Clase [`UpdateActivityRequest`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fc%3A%2FUsers%2Femili%2FDesktop%2Fdemos%2Fteapp%2Fsrc%2Fapp%2FHttp%2FControllers%2FActivityController.php%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A6%2C%22character%22%3A22%7D%7D%5D%2C%22791d55b4-a635-4ed2-86d1-cc2b16a44850%22%5D "Go to definition")
```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class UpdateActivityRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [
            'name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'image' => 'nullable|image',
        ];
    }
}
```
Esta clase valida las solicitudes de actualización de actividades.

## Conclusión
Este tutorial te ha guiado a través de la creación de un CRUD de actividades en Laravel utilizando Blade y Tailwind CSS. Hemos cubierto desde la configuración inicial hasta la implementación de las vistas y controladores necesarios. ¡Felicidades por completar el tutorial!