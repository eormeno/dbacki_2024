### Taller: Crear un Contador Simple en Laravel

#### Objetivo del tutorial:
En este tutorial, aprenderás a desarrollar una pequeña aplicación de contador en Laravel que permite incrementar y decrementar un número utilizando rutas y vistas. Nos enfocaremos en conceptos básicos de Laravel como **rutas**, **vistas blade**, y el uso de **componentes** y **enlaces dinámicos**.

### Requisitos previos:
1. Tener instalado Laravel y configurado un entorno de desarrollo local (con **PHP**, **Composer**, y **Node.js**).
2. Conocimientos básicos de Laravel, Blade, y manejo de rutas.
3. Haber ejecutado el comando `php artisan serve` para lanzar tu servidor local.

#### Paso 1: Configuración de las Rutas

En **Laravel**, las rutas son responsables de definir cómo los usuarios pueden interactuar con tu aplicación. En nuestro caso, queremos definir tres rutas:
- Una para mostrar el contador inicial.
- Otra para incrementar el valor del contador.
- Una más para decrementar el valor.

En el archivo `web.php` que se encuentra en la carpeta `routes`, define las siguientes rutas:

```php
// Rutas de la aplicación

Route::get('/contador', function () {
    return view('contador', ['número' => 0]);
})->name('contador');

Route::get('/contador/{número}/inc', function ($número) {
    return view('contador', ['número' => $número + 1]);
})->name('contador.inc');

Route::get('/contador/{número}/dec', function ($número) {
    return view('contador', ['número' => $número - 1]);
})->name('contador.dec');
```

**Explicación**:
- La primera ruta (`/contador`) inicia el contador en cero y muestra la vista `contador`.
- Las otras dos rutas (`/contador/{número}/inc` y `/contador/{número}/dec`) incrementan y decrementan el número según el valor recibido en la URL.

#### Paso 2: Creación de la Vista Blade

Laravel utiliza el motor de plantillas **Blade** para renderizar HTML de manera dinámica. Vamos a crear la vista `contador.blade.php` en la carpeta `resources/views`.

```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Contador</title>
    @vite('resources/css/app.css') <!-- Cargar los estilos de Tailwind CSS -->
</head>

<body class="font-sans antialiased dark:bg-black dark:text-white/50">
    <main class="container mx-auto px-4 py-8">
        <h1 class="text-3xl font-bold mb-4">Contador</h1>
        <div class="flex items-center space-x-4">
            <span class="text-2xl font-bold m-3">{{ $número }}</span> <!-- Mostrar el número -->
            
            <!-- Botón para incrementar -->
            <x-button class="bg-green-500 hover:bg-green-600 m-3"
                onclick="location.href='{{ route('contador.inc', ['número' => $número]) }}'">
                ++
            </x-button>

            <!-- Botón para decrementar -->
            <x-button class="bg-red-500 hover:bg-red-600 m-3"
                onclick="location.href='{{ route('contador.dec', ['número' => $número]) }}'">
                --
            </x-button>
        </div>
    </main>
</body>

</html>
```

**Explicación**:
- Usamos el componente `x-button` para crear botones interactivos que ejecutan una función de redireccionamiento a las rutas de incremento o decremento.
- La variable `{{ $número }}` se pasa a la vista desde la ruta, y se muestra en pantalla como el número actual del contador.
- Se utiliza **Tailwind CSS** para aplicar estilos.

#### Paso 4: Probar la Aplicación

Ahora, ejecuta tu servidor de desarrollo con el comando:

```bash
php artisan serve
```

Accede a [http://127.0.0.1:8000/contador](http://127.0.0.1:8000/contador) en tu navegador. Verás el contador inicializado en cero. Al hacer clic en los botones, el valor del contador se incrementará o disminuirá según corresponda.

### Actividades

1. **Modificación de la lógica del contador**:
   - Agrega una validación en las rutas de incremento y decremento para evitar que el contador baje de cero.
   - ¿Cómo modificarías la lógica para no permitir un número mayor a 10?
2. **Estilos y Diseño**:
   - Personaliza los estilos de la aplicación utilizando Tailwind CSS.
   - Agrega animaciones o efectos visuales a los botones.
3. **Funcionalidades Adicionales**:
    - Implementa un botón de reinicio para volver a cero el contador.
    - Agrega un botón para duplicar el valor actual del contador.
    - Crea un botón para restablecer el contador a un valor específico.

### Conceptos Clave

- **Rutas en Laravel**: Las rutas son las que definen qué URL acceden a qué partes de tu aplicación y qué lógica ejecutan. Más información [aquí](https://laravel.com/docs/routing).
- **Vistas Blade**: Son plantillas que permiten estructurar el HTML y trabajar con variables. Más sobre Blade [aquí](https://laravel.com/docs/blade).
- **Componentes en Blade**: Permiten reutilizar elementos de interfaz de manera más limpia. Lee más sobre componentes [aquí](https://laravel.com/docs/blade#components).

Este tutorial cubre los conceptos básicos para que puedas desarrollar un contador funcional en Laravel. Si quieres expandir tus conocimientos, te recomiendo explorar el uso de **middleware** y **validaciones** en Laravel para controlar mejor el flujo de la aplicación.