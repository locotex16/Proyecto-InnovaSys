# Proyecto InnovaSys - Configuración de Servidor con Ansible

Este proyecto implementa la automatización completa de un servidor interno para la startup ficticia **InnovaSys**, utilizando Ansible y buenas prácticas de DevOps. El servidor provee dos servicios esenciales:

- **Portal de bienvenida interno (Intranet)** mediante Apache.
- **Repositorio de archivos centralizado** mediante Samba.

## Estructura del Proyecto

innovasys-ansible/
├── ansible.cfg
├── inventory/
│ └── hosts
├── playbook.yml
├── roles/
│ ├── apache/
│ │ ├── tasks/
│ │ │ └── main.yml
│ │ ├── templates/
│ │ │ └── index.html.j2
│ │ ├── handlers/
│ │ │ └── main.yml
│ │ └── vars/
│ │ └── main.yml
│ └── samba/
│ ├── tasks/
│ │ └── main.yml
│ ├── handlers/
│ │ └── main.yml
│ └── vars/
│ └── main.yml

markdown
Copiar
Editar

## Requisitos Previos

- Nodo de control con Ansible instalado (Linux Lite).
- Nodo gestionado Ubuntu Server 24.04 accesible por SSH.
- Clave SSH sin contraseña configurada entre ambos nodos.

## ¿Qué hace el Playbook?

1. **Apache (Rol: `apache`)**
   - Instala Apache2.
   - Reemplaza la página por defecto con una personalizada mediante plantilla Jinja2.
   - Activa e inicia el servicio `apache2`.

2. **Samba (Rol: `samba`)**
   - Instala Samba.
   - Crea el directorio `/srv/samba/proyectos` con permisos adecuados.
   - Crea el grupo `desarrolladores` y el usuario `devuser1`.
   - Configura la compartición `[Proyectos]` accesible solo por el grupo `desarrolladores`.
   - Asigna contraseña Samba a `devuser1` usando un comando con módulo `shell`.
   - Activa e inicia el servicio `smbd`.

## Variables Importantes

Las variables están definidas en los archivos `vars/main.yml` de cada rol, e incluyen:

- `nombre_empresa` (ej: "InnovaSys")
- `usuario_samba`: `devuser1`
- `grupo_samba`: `desarrolladores`
- `directorio_samba`: `/srv/samba/proyectos`
- `samba_password`: `Innova.2025`

## Cómo Ejecutar el Proyecto

1. Clonar el repositorio:
```bash
git clone <URL_DEL_REPOSITORIO>
cd innovasys-ansible
Verificar conectividad SSH con el servidor gestionado.

Ejecutar el playbook:

bash
Copiar
Editar
ansible-playbook -i inventory/hosts playbook.yml
Nota: Asegúrate de que el archivo inventory/hosts tenga la IP correcta del servidor Ubuntu y el usuario SSH adecuado.
