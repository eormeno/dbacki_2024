```php
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