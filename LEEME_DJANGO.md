# Entre Chicas — frontend integrado con Django

Este proyecto combina la estructura de ajvivi/evaluacion3 con las páginas, estilos,
imágenes, video y probadores del ZIP frontend_Entrechicas.zip.
Es una copia de trabajo para ejecución local. No se ha publicado ni subido a GitHub.

## Iniciar en Visual Studio Code (Windows / CMD)

1. Extrae el ZIP en una carpeta nueva y abre `evaluacion3_integrado` en VS Code.
   La carpeta abierta debe contener `manage.py`.
2. Usa Python 3.12 y abre Terminal > Nueva terminal > Command Prompt (CMD).
3. Ejecuta, uno por uno:

```bat
py -3.12 -m venv .venv
.venv\Scripts\activate
python -m pip install -r requirements.txt
python manage.py migrate
python manage.py cargar_demo
python manage.py createsuperuser
python manage.py runserver
```

4. Abre http://127.0.0.1:8000/ y http://127.0.0.1:8000/admin/.
5. En Ctrl+Shift+P > Python: Select Interpreter, selecciona `.venv`.
6. Para detener el servidor: Ctrl+C. Para volver a usarlo, activa `.venv` y ejecuta `python manage.py runserver`.

No necesitas Node, Firebase ni el servidor del puerto 3000 para esta versión.
Se usa Django 5.2 y SQLite. No se incluyen usuarios, contraseñas ni una base de datos precargada.
El comando cargar_demo agrega un salón, cuatro servicios y tres productos ficticios.
Puede repetirse sin sobrescribir precios ni información existente.

## Qué está integrado

- Inicio con diseño original, video, imágenes, modo claro/oscuro y modal de reserva.
- Catálogo leído de la base de datos de Django.
- Disponibilidad de la agenda del salón, con intervalos de inicio de 30 minutos,
  duración de cada servicio y horizonte máximo de 180 días.
- Reservas públicas sin cuenta, como en el frontend recibido. Guarda contacto,
  servicio, precio histórico, inicio, fin y estado. Evita cruces en la agenda pública.
- Idempotencia: repetir la misma solicitud no duplica reservas ni pedidos.
- Tienda con filtros, carrito y solicitudes guardadas en Django. Valida cantidades,
  calcula precios en el servidor y comprueba stock al recibir la solicitud.
- Administración de salones, catálogo, productos, reservas y pedidos con Django admin.
- Ayuda por reglas y datos del catálogo: no es IA generativa ni RAG.
- Páginas de color de cabello, uñas, labiales y uñas sobre fotografía.
- Manifest adaptado. No se instala el service worker antiguo ni se promete operación offline.
- Se conservan las dos aplicaciones del repositorio y sus rutas de prueba.

## Administración

Accede con el superusuario creado por ti. En Salons configura nombre, contacto,
apertura/cierre y disponibilidad de probadores demo. Mantén el slug `demo` para
la portada por defecto. Otros salones se abren con `/?salon=SU-SLUG`.

En Servicios cambia nombres, precios, duración y disponibilidad. En Productos
actualiza categorías y stock. Las fotos de producto no se incluyeron en el ZIP:
se mantiene el marcador de fotografía pendiente.

En Reservas puedes cambiar el estado a cancelada para liberar la hora.
Los demás datos son de consulta para evitar alterar el horario sin validación.
En Pedidos cambia el estado según tu revisión. El stock se gestiona manualmente:
una solicitud NO reserva ni descuenta unidades, y confirmar su estado tampoco.
No se envían correos automáticos ni se cobra dinero.

## Diferencias y límites

- El botón anterior de registro de propietarios ahora dice Administración y abre
  Django admin. No hay registro público de salones/clientas ni login Firebase.
- El panel Firebase/Node del ZIP se sustituye por Django admin; no se migran los
  datos remotos. No se accedió a Firestore ni se copiaron sus registros.
- Una agenda por salón: no hay reservas simultáneas por diferentes profesionales,
  feriados, descansos ni horarios distintos por día de semana.
- Los planes son propuestas informativas. No se activan suscripciones ni pagos
  simulados del sistema antiguo. El acceso a probadores es un interruptor demo.
- Los probadores conservan los algoritmos originales. Cámara y detectores externos
  requieren un navegador compatible, permisos e internet; fuera de localhost,
  normalmente requieren HTTPS. No se ha comprobado la precisión con una cámara física.
- La ayuda responde por reglas sencillas; no consulta ChatGPT ni otros proveedores.
- Es una integración académica local, no un despliegue de producción. Antes de publicar:
  configurar una SECRET_KEY privada mediante DJANGO_SECRET_KEY, DEBUG=False,
  hosts y HTTPS; revisar privacidad, control de abuso y concurrencia de administración.

## Dónde editar

- `templates/entrechicas/`: páginas HTML adaptadas con `{% static %}`.
- `static/entrechicas/`: CSS, JS, imágenes y video del frontend.
- `appentrechicas1/models.py`: datos del negocio.
- `appentrechicas1/views.py`: páginas y API JSON compatible con el frontend.
- `appentrechicas1/admin.py`: panel de administración.
- `appentrechicas1/migrations/`: estructura de la base de datos.
- `entrechicas/urls.py`: rutas de páginas, API y administración.
- `appentrechicas2/`: aplicación original conservada.

## Si ya trabajaste en tu copia local

Usa primero esta entrega en una carpeta separada. Para incorporarla a tu repositorio,
compara los cambios de Python y copia `templates/entrechicas/`, `static/entrechicas/`,
las migraciones y el comando de demo. No reemplaces tu base de datos ni tu entorno virtual.
Si ya definiste otros modelos/migraciones localmente, integra ambos esquemas antes de migrar.
Esta entrega parte de la versión remota revisada, cuyos modelos estaban vacíos.

## Verificar

```bat
python manage.py check
python manage.py test
python manage.py makemigrations --check --dry-run
```

Las pruebas cubren páginas, permisos de administración, CSRF, reservas, superposición,
reintentos, cancelación, pedidos, cantidades y carga no destructiva de datos demo.

Validación de esta entrega: siete pruebas Django aprobadas, comprobación del sistema sin
advertencias, migraciones consistentes y referencias estáticas/JavaScript verificadas.
No se completó una prueba visual en navegador: el navegador de pruebas no pudo instalarse
en el entorno de preparación. Verifica el aspecto y la cámara localmente.
