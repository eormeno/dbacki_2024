# Upload de imágenes y almacenamiento en Laravel
Los siguientes ejemplos constituyen una guía básica sobre cómo manejar la subida, almacenamiento, edición y visualización de archivos, tanto referenciando el nombre del archivo en la tabla como almacenando su contenido en Base64.

## Ventajas y desventajas de ambas opciones
- Almacenamiento del nombre del archivo en la tabla
  - Ventajas
    - Los archivos se almacenan en el sistema de archivos del servidor, lo que reduce el tamaño de la base de datos.
    - Los archivos se pueden servir directamente desde el sistema de archivos, lo que puede ser más eficiente.
  - Desventajas
    - Se necesita un sistema de archivos accesible para almacenar los archivos.
    - Se necesita un mecanismo de limpieza para eliminar los archivos no utilizados.

- Almacenamiento del archivo en Base64
    - Ventajas
        - Los archivos se almacenan directamente en la base de datos, lo que facilita la copia de seguridad y la portabilidad.
        - No se necesita un sistema de archivos accesible para almacenar los archivos.
    - Desventajas
        - Los archivos almacenados en Base64 pueden ocupar más espacio en la base de datos.
        - La recuperación de los archivos puede ser menos eficiente que servirlos directamente desde el sistema de archivos.

## 1. Subida de Archivos y Almacenamiento del Nombre en la Tabla

### Vista con Blade y Tailwind CSS
```html
<form action="/upload" method="POST" enctype="multipart/form-data" class="space-y-4">
    @csrf
    <div>
        <label for="file" class="block text-sm font-medium text-gray-700">Archivo</label>
        <input type="file" name="file" id="file" class="mt-1 block w-full border-gray-300 shadow-sm sm:text-sm focus:ring-indigo-500 focus:border-indigo-500">
    </div>
    <div>
        <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded">Subir</button>
    </div>
</form>
```

### Controlador
```php
public function upload(Request $request)
{
    $request->validate([
        'file' => 'required|file|mimes:jpg,png,jpeg,gif|max:2048',
    ]);

    // Almacenar el archivo en la carpeta 'uploads'
    $fileName = time() . '.' . $request->file->extension();
    $request->file->move(public_path('uploads'), $fileName);

    // Guardar el nombre del archivo en la base de datos
    FileModel::create(['filename' => $fileName]);

    return back()->with('success', 'Archivo subido correctamente.');
}
```

### Migración
```php
Schema::create('files', function (Blueprint $table) {
    $table->id();
    $table->string('filename');
    $table->timestamps();
});
```

## 2. Visualización de Imágenes

### Vista para Mostrar Imágenes
```html
@foreach($files as $file)
    <div class="flex flex-col items-center space-y-2">
        <img src="{{ asset('uploads/' . $file->filename) }}" alt="Imagen" class="w-32 h-32 object-cover">
        <p>{{ $file->filename }}</p>
    </div>
@endforeach
```

### Controlador para Mostrar Imágenes
```php
public function showFiles()
{
    $files = FileModel::all();
    return view('show-files', compact('files'));
}
```

## 3. Subida de Archivos y Almacenamiento del Archivo en Base64

### Controlador
```php
public function uploadBase64(Request $request)
{
    $request->validate([
        'file' => 'required|file|mimes:jpg,png,jpeg,gif|max:2048',
    ]);

    // Obtener el contenido del archivo y convertirlo a Base64
    $file = $request->file('file');
    $base64 = base64_encode(file_get_contents($file));

    // Guardar la cadena Base64 en la base de datos
    FileModel::create(['base64_data' => $base64]);

    return back()->with('success', 'Archivo subido en formato Base64.');
}
```

### Migración para Almacenar Base64
```php
Schema::create('files', function (Blueprint $table) {
    $table->id();
    $table->longText('base64_data');
    $table->timestamps();
});
```

### Vista para Mostrar Imagen en Base64
```html
@foreach($files as $file)
    <div class="flex flex-col items-center space-y-2">
        <img src="data:image/png;base64,{{ $file->base64_data }}" alt="Imagen" class="w-32 h-32 object-cover">
    </div>
@endforeach
```

## 4. Edición de Imágenes

### Vista de Edición
```html
<form action="/edit/{{ $file->id }}" method="POST" enctype="multipart/form-data" class="space-y-4">
    @csrf
    @method('PUT')
    <div>
        <label for="file" class="block text-sm font-medium text-gray-700">Editar Archivo</label>
        <input type="file" name="file" id="file" class="mt-1 block w-full">
    </div>
    <div>
        <button type="submit" class="px-4 py-2 bg-green-500 text-white rounded">Guardar Cambios</button>
    </div>
</form>
```

### Controlador de Edición
```php
public function edit(Request $request, FileModel $file)
{
    $request->validate([
        'file' => 'nullable|file|mimes:jpg,png,jpeg,gif|max:2048',
    ]);

    if ($request->hasFile('file')) {
        $fileName = time() . '.' . $request->file->extension();
        $request->file->move(public_path('uploads'), $fileName);

        // Actualizar el nombre del archivo en la base de datos
        $file->update(['filename' => $fileName]);
    }

    return back()->with('success', 'Imagen actualizada.');
}
```