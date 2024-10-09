```php
<!-- resources/views/patients/index.blade.php -->
<x-event-layout>
    <x-slot name="title">Pacientes</x-slot>
    <div class="py-6">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                <div class="p-6 sm:px-20 bg-white border-b border-gray-200">
                    <div class="mt-4">
                        <a href="{{ route('patients.create') }}"
                            class="inline-flex items-center px-4 py-2 mb-2 bg-indigo-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-indigo-700 active:bg-indigo-900 focus:outline-none focus:border-indigo-900 focus:ring ring-indigo-300 disabled:opacity-25 transition ease-in-out duration-150">
                            Nuevo paciente
                        </a>
                        <div class="overflow-x-hidden">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Código</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Apellidos</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Nombres</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            DNI</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Fecha de nacimiento</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Sexo</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Teléfono</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Email</th>
                                        <th scope="col"
                                            class="px-4 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                                            Acciones</th>
                                    </tr>
                                </thead>
                                <tbody class="bg-white divide-y divide-gray-200">
                                    @foreach ($patients as $key => $patient)
                                        <tr class="{{ $key % 2 === 0 ? 'bg-gray-100' : 'bg-white' }} hover:bg-gray-200">
                                            <td class="px-4 py-2 whitespace-nowrap text-sm font-medium text-gray-900">
                                                {{ $patient->codigo }}</td>
                                            <td class="px-4 py-2 whitespace-nowrap text-sm font-medium text-gray-900">
                                                {{ $patient->apellidos }}</td>
                                            <td class="px-4 py-2 whitespace-nowrap text-sm font-medium text-gray-900">
                                                {{ $patient->nombres }}</td>
                                            <td class="px-4 py-2 whitespace-nowrap text-sm font-medium text-gray-900">
                                                {{ $patient->dni }}</td>
                                            <td
                                                class="px-4 py-2 whitespace-nowrap text-sm text-gray-500 hidden md:table-cell">
                                                {{ $patient->nacimiento }}</td>
                                            <td
                                                class="px-4 py-2 whitespace-nowrap text-sm text-gray-500 hidden md:table-cell">
                                                {{ $patient->sexo }}</td>
                                            <td
                                                class="px-4 py-2 whitespace-nowrap text-sm text-gray-500 hidden md:table-cell">
                                                {{ $patient->telefono }}</td>
                                            <td
                                                class="px-4 py-2 whitespace-nowrap text-sm text-gray-500 hidden md:table-cell">
                                                {{ $patient->email }}</td>

                                            <td class="px-4 py-2 whitespace-nowrap text-sm text-gray-500">
                                                <div class="flex items-center space-x-2">
                                                    <a href="{{ route('patients.show', $patient) }}"
                                                        class="text-indigo-600 hover:text-indigo-900">
                                                        <svg xmlns="http://www.w3.org/2000/svg" fill="none"
                                                            viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
                                                            class="size-6">
                                                            <path stroke-linecap="round" stroke-linejoin="round"
                                                                d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" />
                                                            <path stroke-linecap="round" stroke-linejoin="round"
                                                                d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
                                                        </svg>
                                                    </a>
                                                    <a href="{{ route('patients.edit', $patient) }}"
                                                        class="text-indigo-600 hover:text-indigo-900">
                                                        <svg xmlns="http://www.w3.org/2000/svg" fill="none"
                                                            viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
                                                            class="size-6">
                                                            <path stroke-linecap="round" stroke-linejoin="round"
                                                                d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                                                        </svg>
                                                    </a>
                                                    <form action="{{ route('patients.destroy', $patient) }}"
                                                        method="POST">
                                                        @csrf
                                                        @method('DELETE')
                                                        <button type="submit">
                                                            <div class="text-red-700 hover:text-red-900">
                                                                <svg xmlns="http://www.w3.org/2000/svg" fill="none"
                                                                    viewBox="0 0 24 24" stroke-width="1.5"
                                                                    stroke="currentColor" class="size-6">
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
                    </div>
                    <div class="mt-6">
                        {{ $patients->links() }}
                    </div>

                </div>
            </div>
        </div>
    </div>
</x-event-layout>
```