```php
<!-- resources/views/patients/create.blade.php -->
<x-event-layout>
    <x-slot name="title">Nuevo Paciente</x-slot>
    <div class="py-6">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                <div class="p-6 sm:px-20 bg-white border-b border-gray-200">

                    <a href="{{ route('patients.index') }}">
                        <div class="inline-flex items-center px-4 py-2 mb-2 bg-indigo-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-indigo-700 active:bg-indigo-900 focus:outline-none focus:border-indigo-900 focus:ring ring-indigo-300 disabled:opacity-25 transition ease-in-out duration-150">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M10.5 19.5 3 12m0 0 7.5-7.5M3 12h18" />
                              </svg>
                        </div>
                    </a>

                    <x-validation-errors class="mb-4" />

                    <form action="{{ route('patients.store') }}" method="POST" class="mt-2">
                        @csrf
                        <div class="mt-2">
                            <x-label for="codigo" value="Código" />
                            <x-input id="codigo" class="block mt-1 w-full" type="text" name="codigo"
                                :value="old('codigo')" required autofocus autocomplete="codigo" />
                        </div>
                        <div class="mt-2">
                            <x-label for="apellidos" value="Apellidos" />
                            <x-input id="apellidos" class="block mt-1 w-full" type="text" name="apellidos"
                                :value="old('apellidos')" required autocomplete="apellidos" />
                        </div>
                        <div class="mt-2">
                            <x-label for="nombres" value="Nombres" />
                            <x-input id="nombres" class="block mt-1 w-full" type="text" name="nombres"
                                :value="old('nombres')" required autocomplete="nombres" />
                        </div>
                        <div class="mt-2">
                            <x-label for="dni" value="DNI" />
                            <x-input id="dni" class="block mt-1 w-full" type="text" name="dni"
                                :value="old('dni')" required autocomplete="dni" />
                        </div>
                        <div class="mt-2">
                            <x-label for="nacimiento" value="Fecha de nacimiento" />
                            <x-input id="nacimiento" class="block mt-1 w-full" type="date" name="nacimiento"
                                :value="old('nacimiento')" required autocomplete="nacimiento" />
                        </div>
                        <div class="mt-2">
                            <x-label for="sexo" value="Sexo" />
                            <select name="sexo" required
                                class="border-gray-300 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-300 focus:border-indigo-500 dark:focus:border-indigo-600 focus:ring-indigo-500 dark:focus:ring-indigo-600 rounded-md shadow-sm block mt-1 w-full">
                                <option value="M">Masculino</option>
                                <option value="F">Femenino</option>
                            </select>
                        </div>
                        <div class="mt-2">
                            <x-label for="telefono" value="Teléfono" />
                            <x-input id="telefono" class="block mt-1 w-full" type="text" name="telefono"
                                :value="old('telefono')" required autocomplete="telefono" />
                        </div>
                        <div class="mt-2">
                            <x-label for="email" value="Email" />
                            <x-input id="email" class="block mt-1 w-full" type="email" name="email"
                                :value="old('email')" required autocomplete="email" />
                        </div class="mt-2">
                        <div class="mt-2">
                            <x-label for="direccion" value="Dirección" />
                            <x-input id="direccion" class="block mt-1 w-full" type="text" name="direccion"
                                :value="old('direccion')" required autocomplete="direccion" />
                        </div>
                        <div class="mt-2">
                            <x-label for="observaciones" value="Observaciones" />
                            <textarea name="observaciones" id="observaciones" cols="30" rows="5"
                                class="border-gray-300 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-300 focus:border-indigo-500 dark:focus:border-indigo-600 focus:ring-indigo-500 dark:focus:ring-indigo-600 rounded-md shadow-sm block mt-1 w-full"
                                required>{{ old('observaciones') }}</textarea>
                        </div>
                        <div class="flex items-center justify-end mt-4">
                            <x-button class="ms-4">Crear paciente </x-button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-event-layout>
```