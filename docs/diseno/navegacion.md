# Navegacion

## Definición de Navegación entre Vistas

## Vistas Públicas (Sin autenticación requerida)

Las siguientes vistas están disponibles para todos los usuarios sin necesidad de autenticación:

### Página Principal (`/`)
Vista de inicio que presenta la plataforma y sus características principales.

### Registro (`/register`)
Formulario de registro para nuevos usuarios, tanto freelancers como clientes.

### Inicio de Sesión (`/login`)
Formulario de autenticación para usuarios existentes.

### Galería de Publicaciones (`/services`)
Vista pública que muestra todos los servicios disponibles ofrecidos por freelancers.

### Vista de Detalle de Publicación (`/services/:id`)
Página detallada de un servicio específico con información completa del freelancer y el servicio ofrecido.

### Recuperar Contraseña (`/forgot-password`)
Formulario para recuperación de contraseña mediante email.

## Vistas Privadas (Requieren autenticación)

Las siguientes vistas requieren que el usuario esté autenticado en el sistema:

### Dashboard (`/dashboard`)
Panel principal del usuario autenticado con resumen de actividades y accesos rápidos.

### Mi Perfil (`/profile`)
Vista del perfil personal del usuario con información detallada y estadísticas.

### Crear Publicación (`/services/create`)
Formulario para que los freelancers creen nuevos servicios.

### Editar Publicación (`/services/edit/:id`)
Formulario de edición para servicios existentes del freelancer.

### Mis Servicios (`/my-services`)
Lista de todos los servicios creados por el freelancer actual.

### Crear Proyecto (`/projects/create`)
Formulario para que los clientes publiquen nuevos proyectos.

### Mis Proyectos (`/my-projects`)
Lista de proyectos creados por el cliente actual.

### Galería de Proyectos (`/projects`)
Vista de todos los proyectos disponibles para freelancers.

### Detalle de Proyecto (`/projects/:id`)
Información detallada de un proyecto específico.

### Propuestas Enviadas (`/proposals/sent`)
Lista de propuestas enviadas por freelancers a diferentes proyectos.

### Propuestas Recibidas (`/proposals/received`)
Lista de propuestas recibidas por clientes en sus proyectos.

### Mensajes (`/messages`)
Sistema de mensajería interna entre usuarios.

### Configuración de Cuenta (`/settings`)
Panel de configuración personal del usuario.

## Flujo de Navegación

### Para Freelancers

```
Página Principal → [Login] → Dashboard → Mi Perfil
                             ↓
                           Crear Servicio
                             ↓
                           Mis Servicios
                             ↓
                           Galería de Proyectos → Detalle Proyecto → Enviar Propuesta
```

**Descripción del flujo:**
1. El freelancer ingresa desde la página principal
2. Se autentica mediante login
3. Accede a su dashboard personalizado
4. Completa su perfil profesional
5. Crea servicios para ofrecer sus habilidades
6. Gestiona sus servicios desde "Mis Servicios"
7. Explora proyectos disponibles en la galería
8. Envía propuestas a proyectos de interés

### Para Clientes/Startups

```
Página Principal → [Login] → Dashboard → Crear Proyecto
                             ↓
                           Mis Proyectos → Ver Propuestas
                             ↓
                           Galería de Servicios → Detalle Servicio → Contratar
```

**Descripción del flujo:**
1. El cliente ingresa desde la página principal
2. Se autentica mediante login
3. Accede a su dashboard personalizado
4. Crea proyectos describiendo sus necesidades
5. Gestiona sus proyectos y revisa propuestas recibidas
6. Explora servicios disponibles en la galería
7. Contrata freelancers directamente desde sus servicios

### Navegación Común

```
Dashboard → Mensajes → Conversaciones
         → Mi Perfil → Editar Perfil
```

**Funcionalidades compartidas:**
- **Sistema de Mensajería**: Comunicación directa entre freelancers y clientes
- **Gestión de Perfil**: Edición y actualización de información personal
- **Dashboard**: Centro de control personalizado según el tipo de usuario

## Consideraciones de UX

### Protección de Rutas
- Las vistas privadas redirigen automáticamente al login si el usuario no está autenticado
- Después del login exitoso, el usuario es redirigido a su dashboard

### Navegación Contextual
- Los menús y opciones cambian dinámicamente según el tipo de usuario (freelancer/cliente)
- Las breadcrumbs facilitan la orientación dentro de la aplicación

### Estados de Carga
- Todas las transiciones entre vistas incluyen indicadores de carga
- Los datos se precargan cuando es posible para mejorar la experiencia