# Tutorial: Patients CRUD
## Introducción
De acuerdo a lo epecificado por el cliente, los usuarios con rol de médico o terapeuta deben poder gestionar los pacientes de la aplicación. Para ello, vamos a crear un manager que permita realizar operaciones CRUD (Create, Read, Update, Delete) de los pacientes.
## Componentes necesarios
Ejecutamos el siguiente comando artisan para crear todos los componentes necesarios:
```bash
php artisan make:model Patient -cmfrs
```

Este comando creará un modelo, un controlador, una migración, una factoría, una seeder. El switch `-r` hace que el controlador tenga los métodos de CRUD por defecto. Es decir, los métodos `index`, `create`, `store`, `show`, `edit`, `update`, `destroy`.


