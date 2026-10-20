# Sistema de gestión de visitas

Aplicación de escritorio académica en C#, Windows Forms, .NET Framework 4.8 y SQL Server para registrar visitas a edificios y aulas o departamentos.

## Alcance

La interfaz contempla inicio de sesión y pantallas para administración, usuarios generales y registro de visitantes. El modelo incluye usuarios, edificios, aulas y visitas con datos de contacto, horarios, motivo y fotografía.

## Arquitectura

- `Sistema de visitas/CapaPresentación/`: formularios Windows Forms.
- `Sistema de visitas/Capa_Negocio/`: capa de negocio.
- `Sistema de visitas/Capa_Entidad/`: entidades.
- `Sistema de visitas/D_Visitas/`: acceso a datos con ADO.NET y procedimientos almacenados.
- `Sistema de visitas.sql`: creación de base, tablas, procedimientos y datos de ejemplo.

## Requisitos

Windows, Visual Studio con desarrollo de escritorio .NET y el targeting pack de .NET Framework 4.8; SQL Server y una herramienta para ejecutar T-SQL, por ejemplo SSMS.

## Preparación local

1. Clonar el repositorio.
2. Revisar y ejecutar `Sistema de visitas.sql` en una instancia de pruebas. El script crea `Sistema_visitas` e incluye datos de ejemplo; no ejecutarlo sobre una instalación existente sin revisar cada bloque.
3. Abrir `Sistema de visitas/Sistema de visitas.sln`.
4. Cambiar el servidor de la cadena `conect` en `Sistema de visitas/CapaPresentación/App.config` por la instancia local. Mantener `Database=Sistema_visitas` y configurar la autenticación que corresponda.
5. Establecer el proyecto de presentación como inicio, compilar y ejecutar.
6. Revisar los usuarios de ejemplo del script y sustituir sus contraseñas antes de cualquier uso compartido.

La configuración actual usa autenticación integrada de Windows; el usuario que ejecuta la aplicación necesita permisos en SQL Server.

## Comprobación manual sugerida

Probar inicio de sesión válido e inválido, permisos de administrador y usuario general, alta de edificios y aulas y registro de una visita. Verificar que el aula pertenezca al edificio seleccionado y que los horarios sean coherentes.

## Limitaciones y mejoras

- El script y la autenticación manejan contraseñas sin un mecanismo de hash visible en el flujo revisado. Implementar almacenamiento y verificación mediante un algoritmo de hash de contraseñas antes de un uso real.
- Comprobar autorización también en las operaciones de datos, además de ocultar opciones en la interfaz.
- Reforzar coherencia entre edificio y aula y validación de entrada/salida.
- Sustituir conexiones compartidas por recursos con `using` y parámetros con tipos explícitos.
- Incorporar pruebas de permisos e integridad y capturas de las pantallas.
- Revisar archivos generados versionados y mantener un `.gitignore` adecuado para Visual Studio.

## Validación

Revisión estática del proyecto de presentación, acceso a datos, configuración y script SQL. No se compiló en Windows ni se ejecutó la base de datos. La existencia de un workflow no certifica una compilación correcta.

## Licencia

Consultar el archivo [LICENSE](LICENSE).

## Pantallas y flujo de trabajo

| Pantalla | Función observada |
| --- | --- |
| `P_Login` | Autenticar y abrir la interfaz según Administrador o General |
| `P_admin` | Gestión administrativa de registros |
| `P_General` | Consulta de visitas asociadas a edificios del encargado |
| `P_RegistroVisitantes` | Captura de visitante, ubicación, horarios, motivo y fotografía |

El enlace de registro también está disponible desde el login. El flujo General pasa las credenciales a la consulta `SP_ObtenerVisitaEncargado`; la segmentación visible no debe confundirse con una autorización integral ya auditada.

Preparar primero usuarios, edificios con encargado y aulas asociadas. Después registrar una visita y comprobar su aparición en los listados. Probar con usuario General y Administrador para revisar el alcance de cada pantalla.

## Modelo de datos

| Tabla | Relaciones y contenido |
| --- | --- |
| `Usuarios` | Datos, tipo de usuario y nombre calculado |
| `Edificios` | Encargado relacionado con Usuarios |
| `Aulas` | Edificio al que pertenece cada aula |
| `Visitantes` | Datos personales, edificio, aula opcional, horarios, motivo y foto |

El nombre de acceso se calcula con las iniciales de nombre/apellido y el ID formateado. Consultar los usuarios generados en una base de pruebas; no copiar contraseñas del seed a documentación ni usarlas en entornos compartidos.

Las fotografías se guardan como `VARBINARY(MAX)` en SQL Server. Respaldar la base conserva esa información. Los procedimientos almacenados incluyen familias de operaciones de usuarios, edificios, aulas y visitas, además de autenticación y consulta por encargado.

## Configuración y diagnóstico

La cadena `conect` se encuentra en `Sistema de visitas/CapaPresentación/App.config`, no en una variable de entorno. Ejemplo sin contraseña:

```xml
<connectionStrings>
  <add name="conect"
       connectionString="Server=SERVIDOR\INSTANCIA;Database=Sistema_visitas;Integrated Security=True;" />
</connectionStrings>
```

| Problema | Qué revisar |
| --- | --- |
| No conecta | Servicio SQL Server, instancia y permisos del usuario de Windows |
| Procedimiento inexistente | Base correcta y bloques SQL ejecutados |
| Login rechazado | Usuario calculado, contraseña local de prueba y tipo válido |
| Listado General vacío | Relación entre encargado, edificio y visitas |
| Falta Framework 4.8 | Instalar targeting pack para compilar |

El script es de preparación inicial y contiene datos y consultas de ejemplo. No ejecutarlo indiscriminadamente de nuevo contra una instalación existente. Revisar sintaxis, lotes y dependencias antes de correrlo. No se publica directamente como web: es una aplicación WinForms para Windows.

Esta ampliación contrasta pantallas, configuración, solución y SQL; no certifica compilación, permisos completos ni ejecución real en Windows.

## Autor

[Jeison Rosario](https://github.com/Jeison-Rosario)
