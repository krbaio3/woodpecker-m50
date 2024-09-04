# Woodpecker CI/CD
## Introduccion 
## Explicación de la configuración:
- Woodpecker Server (woodpecker-server):
Este servicio es el servidor que gestiona la interfaz web de Woodpecker y coordina la ejecución de los pipelines.

- La variable WOODPECKER_AGENT_SECRET se usa para la autenticación entre el servidor y los agentes.
WOODPECKER_ADMIN es el nombre de usuario administrador que tendrá acceso completo a la interfaz.

- WOODPECKER_OPEN permite que el sistema esté abierto a nuevos usuarios (puedes deshabilitarlo en producción).

- WOODPECKER_GITEA indica que está integrado con Gitea, un sistema de gestión de código similar a GitHub. Si estás usando otro servicio (GitHub, GitLab, etc.), puedes cambiar esta configuración.

## Woodpecker Agent (woodpecker-agent):

- El agente ejecuta los pipelines de CI/CD.
- El agente debe autenticarse con el servidor utilizando el WOODPECKER_AGENT_SECRET.
- El volumen docker.sock permite que el agente ejecute contenedores Docker como parte de los pipelines.

## Levantar los servicios

```bash
docker-compose up -d
```

Esto levantará el servidor Woodpecker en el puerto 8000 y el agente Docker estará listo para ejecutar los pipelines.

## Configurar la integración con Gitea (opcional)

Si estás usando Gitea como sistema de gestión de código, debes agregar la aplicación en Gitea para permitir la integración:

- Ve a la sección de Configuración de aplicaciones en tu instancia de Gitea.
- Crea una nueva aplicación OAuth y proporciona la siguiente configuración:
  - Redirect URI: http://your-woodpecker-server:8000/login
  - Client ID y Client Secret: Guarda estos valores y configúralos en el archivo docker-compose.yml en las variables WOODPECKER_GITEA_CLIENT y WOODPECKER_GITEA_SECRET.

## Usar Woodpecker CI
  Una vez que esté levantado, puedes acceder al servidor Woodpecker en http://localhost:8000 (o el dominio/IP que estés usando) e iniciar sesión con la cuenta de administrador.

Podrás empezar a crear pipelines para tus proyectos de código fuente y vincularlos a los repositorios en tu sistema de control de versiones (por ejemplo, Gitea, GitHub, GitLab, etc.).

## Customización

- Otras Integraciones: Si prefieres integrar Woodpecker con GitHub o GitLab, puedes modificar las variables de entorno correspondientes (WOODPECKER_GITHUB, WOODPECKER_GITLAB, etc.).

- Persistencia de datos: La configuración incluye un volumen para almacenar los datos de Woodpecker (./woodpecker-data), asegurando que los datos no se pierdan al reiniciar los contenedores.