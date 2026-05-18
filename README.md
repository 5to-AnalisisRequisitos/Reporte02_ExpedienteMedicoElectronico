# 🏥 EME — Expediente Médico Electrónico

Sistema web para la gestión clínica y administrativa de consultorios médicos pequeños. Permite registrar pacientes, agendar citas, registrar consultas y administrar usuarios con roles diferenciados.

> Proyecto desarrollado para la materia **Análisis de Requisitos** — Ingeniería de Software, UACM.

---

## 📋 Tabla de contenidos

- [Descripción](#descripción)
- [Requisitos previos](#requisitos-previos)
- [Instalación](#instalación)
- [Iniciar el sistema](#iniciar-el-sistema)
- [Primer uso](#primer-uso)
- [Roles de usuario](#roles-de-usuario)
- [Funcionalidades principales](#funcionalidades-principales)
- [Seguridad](#seguridad)
- [Preguntas frecuentes](#preguntas-frecuentes)

---

## Descripción

EME es una aplicación web que corre localmente (`localhost`) y centraliza la información clínica y administrativa de un consultorio:

- Registro de pacientes con expediente clínico único.
- Agendado de citas médicas.
- Registro de consultas con signos vitales, diagnóstico y recetas.
- Tres roles de usuario con permisos diferenciados.
- Auditoría automática de todas las acciones del sistema.

---

## Requisitos previos

| Requisito | Versión mínima | Descarga |
|-----------|---------------|---------|
| Node.js | v18 o superior | https://nodejs.org/es/download/ |
| Navegador web | Reciente | Chrome, Firefox, Edge o Safari |

> `npm` se instala automáticamente junto con Node.js.

Para verificar que Node.js está instalado:

```bash
node --version
# Debe mostrar v18.x.x o superior
```

---

## Instalación

1. Descarga el proyecto:

```
https://github.com/5to-AnalisisRequisitos/Proyecto01_ExpedienteMedicoElectronico/archive/refs/heads/main.zip
```

2. Descomprime el archivo. Debes ver la siguiente estructura:

```
Proyecto01_ExpedienteMedicoElectronico-main/
├── server.js
├── setup.js
├── package.json
├── README.md
├── iniciar.sh
├── iniciar.bat
├── .gitignore
└── public/
    ├── index.html
    ├── styles.css
    └── app.js
```

> ⚠️ No muevas, renombres ni borres ningún archivo. El sistema los necesita todos.

---

## Iniciar el sistema

### Windows

```bat
./iniciar.bat
```

### Linux / macOS

```bash
# Solo la primera vez:
chmod +x iniciar.sh

./iniciar.sh
```

Cuando el servidor esté listo verás:

```
========================================
EME v2.0 - Expediente Medico Electronico
========================================
Servidor: http://localhost:3000
Estado:   Funcionando
```

> ⚠️ No cierres la ventana de la terminal mientras uses el sistema.

Abre tu navegador en: **http://localhost:3000**

---

## Primer uso

La primera vez que abras el sistema deberás crear el **Administrador principal**:

1. Haz clic en **"Crear administrador"**.
2. Completa el formulario (nombre, email, contraseña con mínimo 8 caracteres, una mayúscula, una minúscula y un número).
3. Guarda el **código de recuperación** que se muestra (formato `EME-XXXX-XXXX-XXXX-XXXX`).

> ⚠️ El código aparece **una sola vez**. Guárdalo en un lugar seguro: es la única forma de recuperar tu cuenta si olvidas la contraseña.

Una vez dentro, crea las cuentas del resto del equipo desde **"Gestión de usuarios"**.

---

## Roles de usuario

| Rol | Puede hacer | No puede hacer |
|-----|-------------|----------------|
| **Administrador** | Gestionar cuentas, ver auditoría global, restaurar pacientes eliminados, ver estadísticas. | Ver expedientes, consultas ni datos clínicos. |
| **Médico** | Registrar y gestionar pacientes, atender consultas, recetar, eliminar pacientes. | Crear cuentas de usuario. |
| **Recepcionista** | Registrar pacientes, agendar y cancelar citas, editar datos básicos. | Ver consultas, diagnósticos ni recetas. Tampoco puede eliminar pacientes. |

---

## Funcionalidades principales

### Médico

- **Registrar paciente:** nombre, CURP, fecha de nacimiento, tipo de sangre, alergias, etc. El sistema genera automáticamente un número de expediente único (`EXP-2026-00001`).
- **Buscar paciente:** por nombre, CURP o teléfono.
- **Agendar cita:** selecciona fecha, hora, médico y motivo desde el detalle del paciente.
- **Registrar consulta:** signos vitales, síntomas, diagnóstico, plan de tratamiento y medicamentos prescritos.
- **Eliminar paciente:** eliminación lógica (el expediente se archiva; solo el Administrador puede restaurarlo).

### Recepcionista

Mismos pasos que el Médico para registrar pacientes, buscar y agendar citas. No tiene acceso a información clínica.

### Administrador

- **Gestión de usuarios:** crear, editar, activar o desactivar cuentas.
- **Auditoría global:** filtrar por tipo de acción o email del usuario.
- **Pacientes eliminados:** restaurar registros archivados.

---

## Seguridad

- Sesión con cierre automático tras **15 minutos de inactividad** (aviso con cuenta regresiva de 60 s).
- Bloqueo de cuenta tras **5 intentos de login fallidos** durante 15 minutos.
- Cabeceras de seguridad via **Helmet**.
- CORS restringido a `localhost:3000`.
- Todos los datos se almacenan localmente en `eme.db` (SQLite). Para hacer respaldos, copia ese archivo periódicamente.

---

## Preguntas frecuentes

**El navegador dice "No se puede acceder a localhost:3000"**
El servidor no está corriendo. Ejecuta nuevamente `iniciar.bat` o `iniciar.sh` y no cierres la terminal.

**"Demasiados intentos. Espera 15 minutos"**
Fallaste 5 veces al iniciar sesión. Espera el tiempo indicado y verifica tus credenciales.

**¿Dónde se guardan los datos?**
En el archivo `eme.db` dentro de la carpeta del proyecto. Cópialo regularmente para hacer respaldos.

**¿Puedo cambiar el puerto 3000?**
Sí. Edita el archivo `.env` y cambia `PORT=3000` por el puerto deseado. Reinicia el sistema y accede desde `http://localhost:<puerto>`.

**¿Puedo acceder desde otra computadora en la red?**
No por defecto. El sistema está restringido a `localhost`. Se requieren modificaciones en `server.js` para habilitar acceso en red local.

**Olvidé el código de recuperación del único Administrador**
Deberás reiniciar el sistema:
1. Apaga el servidor (`Ctrl + C`).
2. Haz una copia de `eme.db` como respaldo.
3. Borra `eme.db` de la carpeta del proyecto.
4. Vuelve a iniciar el sistema y crea un nuevo Administrador.

> ⚠️ Esto elimina **todos los datos** (pacientes, citas, consultas). Úsalo solo como último recurso.

---

## Apagar el sistema

1. Asegúrate de que todos los usuarios hayan cerrado sesión.
2. Ve a la terminal donde corre el servidor.
3. Presiona `Ctrl + C` y confirma con `S` o `Y`.

---

## Autores

- Edson Joel Carrera Avila
- Mauricio Bonifacio Hernández

**Grupo:** 302 | **Profesor:** Prof. Mtro. Raul Rolando Jara Montes  
Universidad Autónoma de la Ciudad de México — Licenciatura en Ingeniería de Software
