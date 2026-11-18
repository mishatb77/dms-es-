# Español para DankMaterialShell (DMS)

Traducción al español para **DankMaterialShell (DMS)** en Arch Linux. El
proyecto aplica traducciones adicionales y correcciones al catálogo
español de DMS y, cuando está disponible, a **DCommon**.

El instalador también puede configurar un hook para volver a aplicar la
traducción después de iniciar sesión, ya que DMS extrae su shell en una
ruta temporal y las actualizaciones pueden reemplazar esos archivos.

## Características

-   Traducciones y correcciones para el shell principal de DMS.
-   Traducciones adicionales para cadenas que DMS deja en inglés.
-   Copia al catálogo de DCommon de traducciones que faltan, cuando
    DCommon está presente.
-   Copias de seguridad de los catálogos originales antes de
    modificarlos.
-   Hook opcional para reaplicar la traducción después del inicio de
    sesión.
-   Comandos para comprobar el estado, auditar cadenas pendientes y
    revertir los cambios.

## Compatibilidad

-   **Sistema:** Arch Linux y distribuciones basadas en Arch.
-   **Entorno:** DMS instalado y en ejecución.
-   **Compositor:** el hook incluido está preparado para Hyprland. El
    instalador menciona también niri y mango, pero la integración
    automática del hook utiliza rutas y eventos específicos de Hyprland.
    En otros compositores, utiliza `--no-hook` y adapta la integración
    por separado.
-   **DMS:** el instalador busca la extracción activa en
    `/run/user/<UID>/danklinux-shell/` o `/run/user/<UID>/dms-shell/`.

Este proyecto no instala DMS. Es necesario tener DMS instalado y haberlo
iniciado al menos una vez para que extraiga su shell.

## Requisitos

Para la instalación normal:

-   `python3`
-   `jq`
-   DankMaterialShell instalado y ejecutándose.

Para las opciones de idioma del sistema y paquetes adicionales también
se necesitan:

-   `sudo`
-   `pacman`
-   `locale-gen`

## Instalación

Clona el repositorio:

``` bash
git clone https://github.com/Misha81-oss/dms-es-.git
cd dms-es-
```

Asegúrate de que DMS esté iniciado y ejecuta:

``` bash
./install.sh
```

La instalación normal no necesita `sudo`. El instalador modifica los
catálogos españoles de la extracción activa de DMS y crea copias de
seguridad con el sufijo `.bak-dms-es`.

Si el hook está habilitado, también instala una copia del script y
enlaza la carpeta de traducciones en la configuración del usuario. En
Hyprland, añade la carga del hook a `~/.config/hypr/dms/binds-user.lua`
si ese archivo existe.

Si no ves los cambios inmediatamente, reinicia DMS:

``` bash
systemctl --user restart dms
```

## Opciones del instalador

  -----------------------------------------------------------------------
  Comando                             Función
  ----------------------------------- -----------------------------------
  `./install.sh`                      Aplica la traducción y configura el
                                      hook.

  `./install.sh --no-hook`            Aplica la traducción sin configurar
                                      el hook.

  `./install.sh --check`              Muestra las rutas detectadas, el
                                      estado de los catálogos y la
                                      cobertura.

  `./install.sh --audit`              Muestra hasta 60 cadenas que siguen
                                      sin traducción.

  `./install.sh --revert`             Restaura los catálogos desde las
                                      copias de seguridad y retira la
                                      integración del hook.

  `./install.sh --os`                 Además, configura el locale del
                                      sistema como `es_ES.UTF-8`
                                      (requiere `sudo`).

  `./install.sh --paquetes`           Además, instala paquetes de idioma
                                      para Firefox, diccionarios y
                                      páginas de manual (requiere `sudo`
                                      y `pacman`).

  `./install.sh --todo`               Aplica la traducción, configura el
                                      locale e instala los paquetes de
                                      idioma (requiere `sudo`).

  `./install.sh --help`               Muestra la ayuda breve del
                                      instalador.
  -----------------------------------------------------------------------

### Locale y paquetes opcionales

`--os` configura el locale del sistema como `es_ES.UTF-8` y escribe
`/etc/locale.conf`.

`--paquetes` instala, si faltan, estos paquetes de Arch Linux:

-   `firefox-i18n-es-es`
-   `hunspell-es_es`
-   `hyphen-es`
-   `mythes-es`
-   `man-pages-es`

Estas opciones afectan a todo el sistema y solicitan permisos mediante
`sudo`. No son necesarias para traducir DMS.

## Actualizar la traducción

Entra en la carpeta del repositorio, descarga los cambios y vuelve a
ejecutar el instalador:

``` bash
cd dms-es-
git pull
./install.sh
```

Si el repositorio ya está clonado, no es necesario clonarlo otra vez.

## Comprobar y auditar

Para comprobar el estado de la instalación:

``` bash
./install.sh --check
```

Para listar cadenas que todavía aparecen en inglés y que pueden servir
como referencia para futuras traducciones:

``` bash
./install.sh --audit
```

La cobertura se calcula a partir de las cadenas `I18n.tr(...)`
encontradas en los archivos QML de la extracción activa de DMS.

## Revertir los cambios

Para restaurar los catálogos originales guardados por el instalador:

``` bash
./install.sh --revert
```

Después, reinicia DMS:

``` bash
systemctl --user restart dms
```

La reversión depende de que existan las copias `.bak-dms-es` creadas
durante la instalación. No las elimines si quieres conservar la
posibilidad de restaurar los catálogos originales.

## Estructura del proyecto

``` text
dms-es-/
├── .gitignore
├── LICENSE
├── README.md
├── install.sh
└── translations/
    ├── extras-es.json
    ├── overrides-dankcommon.json
    └── overrides-es.json
```

-   `install.sh`: instalador, comprobación, auditoría y reversión.
-   `translations/overrides-es.json`: traducciones y correcciones para
    el shell principal.
-   `translations/overrides-dankcommon.json`: traducciones específicas
    para DCommon.
-   `translations/extras-es.json`: traducciones adicionales compartidas.
-   `LICENSE`: licencia del proyecto.

## Contribuir

Las contribuciones para corregir traducciones o añadir cadenas nuevas
son bienvenidas.

1.  Haz un fork del repositorio.
2.  Crea una rama para tus cambios.
3.  Modifica los archivos JSON de `translations/`.
4.  Comprueba que los archivos JSON sean válidos.
5.  Envía un Pull Request con una descripción breve de los cambios.

Antes de proponer cambios en el instalador, revisa cuidadosamente las
rutas y las operaciones que realiza.

## Avisos

-   Ejecuta `install.sh` como usuario normal, nunca como `root`.
-   El instalador comprueba que no se ejecute como `root`.
-   La instalación normal no utiliza `sudo`; únicamente `--os` y
    `--paquetes` requieren privilegios administrativos.
-   El hook automático incluido está diseñado para Hyprland. En otros
    compositores, desactívalo con `--no-hook`.
-   Este proyecto es una traducción comunitaria y no forma parte oficial
    de DankMaterialShell.

## Créditos

Proyecto de traducción al español mantenido por
[Misha81-oss](https://github.com/Misha81-oss).

Repositorio: <https://github.com/Misha81-oss/dms-es->
