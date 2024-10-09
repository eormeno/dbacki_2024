```php
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