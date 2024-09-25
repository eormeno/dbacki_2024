# Crear landing page con Laravel, blade y Tailwind CSS
## Introducción
En este documento se muestra cómo crear una landing page con Laravel, blade y Tailwind CSS. Se parte de una vista `welcome` por defecto de Laravel, y se crea una nueva vista llamada `landing` con un diseño más complejo. Se incluyen imágenes y se muestra cómo acceder a ellas desde la vista. Además, se incluye la funcionalidad de ingresar a la aplicación y registrarse en la landing page.

## Contenido
1. [Objetivo](#objetivo)
2. [La ruta de la vista welcome](#la-ruta-de-la-vista-welcome)
3. [Crear la vista landing](#crear-la-vista-landing)
4. [Modificar la ruta de la vista landing](#modificar-la-ruta-de-la-vista-landing)
5. [Acceder a la landing page](#acceder-a-la-landing-page)
6. [Incluir la funcionalidad de la landing page](#incluir-la-funcionalidad-de-la-landing-page)
7. [Acceso a las imágenes](#acceso-a-las-imágenes)
8. [Conclusión](#conclusión)

## Objetivo
Crear una landing page con Laravel, blade y Tailwind CSS.
## La ruta de la vista welcome
La ruta `/` de la aplicación muestra la vista `welcome`. Esta vista es la que se muestra por defecto al acceder a la aplicación. Ésta, está definida en el documento `routes/web.php`.
```php
Route::get('/', function () {
    return view('welcome');
});
```
A su vez, la vista `welcome` se encuentra en la ruta `resources/views/welcome.blade.php`.
El diseño de esta vista es algo complejo, y no se adapta al requisito del usuario. Por lo tanto, se creará una nueva vista llamada `landing-page.blade.php` en la misma carpeta.
## Crear la vista landing
Creamos el documento `landing.blade.php` en la carpeta `resources/views`.
Insertamos el código HTML que queremos que se muestre en la vista. En este caso, se ha creado una landing page para un software destinado a personas con diagnóstico de autismo.
> :point_up_2: **Nota**: Utilizamos Tailwind CSS con Vite para el diseño de la landing page. Para ello, consulta la documentación de [Tailwind CSS](https://tailwindcss.com/docs/guides/laravel)

```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>TEApp</title>
    @vite('resources/css/app.css')
</head>

<body class="font-sans bg-gray-100">
    <header class="bg-blue-600 text-white py-4">
        <div class="container mx-auto px-4">
            <h1 class="text-3xl font-bold">Software para Personas con Diagnóstico de Autismo</h1>
        </div>
    </header>

    <main class="container mx-auto px-4 py-8">
        <section class="mb-12">
            <h2 class="text-2xl font-semibold mb-4">¿Qué es el Trastorno del Espectro Autista (TEA)?</h2>
            <p class="mb-4">El Trastorno del Espectro Autista (TEA) es una condición caracterizada por presentar
                variables alteraciones con un impacto de por vida. Estas manifestaciones son muy variables entre
                individuos y a través del tiempo, acorde al crecimiento y maduración de las personas.</p>
            <img src="{{ asset('images/image_1.jpeg') }}" alt="Representación del espectro autista"
                class="rounded-lg shadow-lg mb-4">
        </section>

        <section class="mb-12">
            <h2 class="text-2xl font-semibold mb-4">Características del Autismo</h2>
            <ul class="list-disc pl-6 mb-4">
                <li>Dificultades en la comunicación</li>
                <li>Dificultades en las interacciones sociales</li>
                <li>Intereses restringidos</li>
                <li>Repetición de comportamientos</li>
                <li>Sensibilidad sensorial</li>
                <li>Dificultades con el cambio</li>
                <li>Habilidades excepcionales en áreas específicas</li>
            </ul>
            <img src="{{ asset('images/image_2.jpeg') }}" alt="Ilustración de características del autismo"
                class="rounded-lg shadow-lg mb-4">
        </section>

        <section class="mb-12">
            <h2 class="text-2xl font-semibold mb-4">Nuestro Software</h2>
            <p class="mb-4">Desarrollamos un software especializado para ayudar a personas con Trastorno del Espectro
                Autista (TEA). Nuestro objetivo es proporcionar una herramienta que facilite la inclusión y mejore la
                calidad de vida de las personas con TEA.</p>
            <h3 class="text-xl font-semibold mb-2">Características del Software:</h3>
            <ul class="list-disc pl-6 mb-4">
                <li>Interfaz amigable e intuitiva</li>
                <li>Personalización según necesidades individuales</li>
                <li>Comunicación visual</li>
                <li>Estructura y rutina claras</li>
                <li>Accesibilidad en diferentes dispositivos</li>
                <li>Colaboración con expertos en TEA</li>
                <li>Privacidad y seguridad garantizadas</li>
                <li>Actualizaciones y soporte continuo</li>
            </ul>
            <img src="{{ asset('images/image_3.jpeg') }}" alt="Captura de pantalla del software"
                class="rounded-lg shadow-lg mb-4">
        </section>

        <section class="mb-12">
            <h2 class="text-2xl font-semibold mb-4">Beneficios de Nuestro Software</h2>
            <ul class="list-disc pl-6 mb-4">
                <li>Desarrollo de habilidades cognitivas, emocionales y motrices</li>
                <li>Fomento de actividades interactivas para mejorar relaciones interpersonales</li>
                <li>Complemento a la intervención terapéutica tradicional</li>
                <li>Contenido adaptable y personalizable</li>
                <li>Retroalimentación inmediata y seguimiento del progreso</li>
            </ul>
            <img src="{{ asset('images/image_4.jpeg') }}" alt="Personas utilizando el software"
                class="rounded-lg shadow-lg mb-4">
        </section>

        <section class="bg-blue-100 p-6 rounded-lg">
            <h2 class="text-2xl font-semibold mb-4">Contáctenos</h2>
            <p>Para más información sobre nuestro software o para solicitar una demostración, por favor contáctenos:</p>
            <a href="mailto:info@autismosoftware.com" class="text-blue-600 hover:underline">info@autismosoftware.com</a>
        </section>
    </main>

    <footer class="bg-gray-800 text-white py-4 mt-12">
        <div class="container mx-auto px-4 text-center">
            <p>&copy; 2024 Software para Personas con Diagnóstico de Autismo. Todos los derechos reservados.</p>
        </div>
    </footer>
</body>
</html>
```
## Modificar la ruta de la vista landing
Para que la vista `landing.blade.php` se muestre al acceder a la ruta `/`, se debe modificar el archivo `routes/web.php`.
```php
Route::get('/', function () {
    return view('landing');
});
```
## Acceder a la landing page
Al acceder a la ruta `/`, se mostrará la vista `landing.blade.php` en lugar de la vista `welcome.blade.php`.
## Incluir la funcionalidad de la landing page
La vista `landing.blade.php` no tiene las funcionaliades de ingresar a la aplicación ni la de registrarse. Para ello, se deben incluir el siguiente código blade en el header de la vista.
```php
    @if (Route::has('login'))
        <nav class="-mx-3 flex flex-1 justify-end">
            @auth
                <a href="{{ url('/dashboard') }}"
                    class="rounded-md px-3 py-2 text-black ring-1 ring-transparent transition hover:text-black/70 focus:outline-none focus-visible:ring-[#FF2D20] dark:text-white dark:hover:text-white/80 dark:focus-visible:ring-white">Panel</a>
            @else
                <a href="{{ route('login') }}"
                    class="rounded-md px-3 py-2 text-black ring-1 ring-transparent transition hover:text-black/70 focus:outline-none focus-visible:ring-[#FF2D20] dark:text-white dark:hover:text-white/80 dark:focus-visible:ring-white">Ingresar</a>

                @if (Route::has('register'))
                    <a href="{{ route('register') }}"
                        class="rounded-md px-3 py-2 text-black ring-1 ring-transparent transition hover:text-black/70 focus:outline-none focus-visible:ring-[#FF2D20] dark:text-white dark:hover:text-white/80 dark:focus-visible:ring-white">
                        Registrarse
                    </a>
                @endif
            @endauth
        </nav>
    @endif
```

## Acceso a las imágenes
Las imágenes que se muestran en la vista `landing.blade.php` se deben ubicar en la carpeta `public/images`. Para acceder a ellas, se debe utilizar la función `asset()` de Laravel.
```html
<img src="{{ asset('images/image_1.jpeg') }}" alt="Representación del espectro autista"
    class="rounded-lg shadow-lg mb-4">
```
Copia las imágenes de este documento en la carpeta `public/images` de su proyecto Laravel.

También puedes cambiar el logo de la página de login y registro, ubicado en la carpeta `public/images/app-logo.png` por el provisto en este documento.

La imagen `app-logo.png` está definida el componente blade `resources/views/components/authentication-card-logo.blade.php`.
```php
<div>
    <img src="{{ asset('images/app-logo.png') }}" alt="Logo" class="w-32 h-32" />
</div>
```

## Conclusión
En este documento se ha mostrado cómo crear una landing page con Laravel, blade y Tailwind CSS. Se ha creado una vista `landing` con un diseño más complejo que la vista `welcome` por defecto de Laravel. Se ha incluido la funcionalidad de ingresar a la aplicación y registrarse en la landing page. Además, se ha mostrado cómo acceder a las imágenes desde la vista.