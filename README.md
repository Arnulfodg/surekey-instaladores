# SureKey — Instaladores

Este repositorio contiene **únicamente los paquetes `.deb` listos para
instalar** de SureKey. El código fuente no está publicado aquí.

## ¿Qué es SureKey?

SureKey es un gestor de contraseñas **local, cifrado y de un solo
usuario**. Corre en tu propia máquina (una Raspberry Pi, una PC, un
servidor casero) — nunca en la nube de un tercero — y lo accedés desde
el navegador, típicamente a través de tu propia red privada
[Tailscale](https://tailscale.com), así que solo vos podés llegar a él,
esté donde esté.

Qué incluye:

- **Bóveda de credenciales**: sitios con múltiples usuarios/contraseñas
  por sitio, notas cifradas, generador de contraseñas.
- **Cifrado real**: la contraseña maestra nunca se guarda en disco; cada
  contraseña y nota se cifra individualmente con AES-256-GCM, derivado
  con Argon2id. Ni siquiera con acceso al archivo de base de datos se
  puede leer nada sin la contraseña maestra.
- **Recuperación de contraseña maestra** mediante preguntas de
  seguridad — si la olvidás, no perdés la bóveda.
- **Segundo factor (TOTP)** opcional, compatible con cualquier app
  autenticadora (Google Authenticator, Authy, etc.).
- **Backup y restauración** cifrados, exportables como un solo archivo.
- **Interfaz en español e inglés**, pensada para usarse cómodamente
  desde el celular.

Es gratis y de código cerrado en este repositorio — ver la sección de
[Licencia](#licencia) más abajo.

## Cómo se instala

1. **Elegí el paquete correcto para tu equipo**, en la
   [versión más reciente](https://github.com/Arnulfodg/surekey-instaladores/releases/latest):

   | Arquitectura | Para qué equipo |
   |---|---|
   | `amd64` | PC o servidor normal (Intel/AMD, 64 bits) |
   | `arm64` | Raspberry Pi 4 o 5 con el sistema operativo de 64 bits (el caso más común hoy) |
   | `armhf` | Raspberry Pi más antigua, o Raspberry Pi OS de 32 bits |

   Si no sabés cuál te corresponde en una Raspberry Pi, corré `uname -m`:
   `aarch64` → `arm64`, `armv7l` → `armhf`.

2. **Descargá el `.deb`** desde la terminal, con el comando de tu
   arquitectura (versión actual: **v0.3.8**):

   ```bash
   # amd64 — PC o servidor (Intel/AMD, 64 bits)
   curl -fLO https://github.com/Arnulfodg/surekey-instaladores/releases/download/v0.3.8/surekey_0.3.8_amd64.deb

   # arm64 — Raspberry Pi 4 o 5 con sistema de 64 bits
   curl -fLO https://github.com/Arnulfodg/surekey-instaladores/releases/download/v0.3.8/surekey_0.3.8_arm64.deb

   # armhf — Raspberry Pi más antigua, o sistema de 32 bits
   curl -fLO https://github.com/Arnulfodg/surekey-instaladores/releases/download/v0.3.8/surekey_0.3.8_armhf.deb
   ```

   El archivo queda en la carpeta donde corriste el comando. Si preferís
   el navegador, los mismos archivos están en la página de
   [Releases](https://github.com/Arnulfodg/surekey-instaladores/releases/latest).

3. **Instalalo**:

   ```bash
   sudo apt install ./surekey_<version>_<arquitectura>.deb
   ```

   Esto instala SureKey como servicio del sistema (arranca solo, incluso
   después de reiniciar el equipo) corriendo como un usuario dedicado
   `surekey`, con sus datos guardados en `/var/lib/surekey` — protegidos
   por permisos del sistema operativo, accesibles solo por ese usuario y
   por root.

4. **(Opcional, recomendado) Hacelo accesible por Tailscale**, para
   poder entrar desde tu celular o cualquier otro dispositivo tuyo sin
   exponer nada a Internet:

   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh   # si no lo tenés instalado
   sudo tailscale up                                   # login único, interactivo
   sudo tailscale serve --bg http://127.0.0.1:8787
   ```

   A partir de ahí, SureKey queda disponible en una URL propia con HTTPS
   automático (algo como `https://<tu-equipo>.<tu-tailnet>.ts.net`), sin
   necesidad de abrir puertos ni configurar certificados a mano.

**Verificá la integridad del archivo descargado** (opcional, pero
recomendado) comparándolo contra el `SHA256SUMS` de la misma carpeta:

```bash
sha256sum -c SHA256SUMS
```

## Cómo se usa

1. Abrí la dirección donde quedó publicado SureKey (`http://<ip-del-equipo>:8787`
   en tu red local, o la URL de Tailscale si lo configuraste) desde el
   navegador.
2. La primera vez, vas a pasar por una **configuración inicial**: tu
   nombre, la contraseña maestra que vas a usar para entrar (elegí una
   fuerte — es la única que vas a necesitar recordar), y dos preguntas
   de seguridad para poder recuperar el acceso si alguna vez olvidás la
   contraseña.
3. Después de eso, cada vez que entres vas a iniciar sesión con esa
   contraseña maestra. Desde ahí podés agregar sitios y credenciales, y
   activar el segundo factor (TOTP) desde "Administración".
4. Actualizar a una versión nueva es tan simple como repetir el paso 3
   de instalación con el `.deb` más reciente — `apt` se encarga de
   reemplazar el binario y reiniciar el servicio solo.

## Licencia

SureKey se distribuye bajo la **SureKey Noncommercial License**: es
libre de usar, copiar y redistribuir para uso personal, educativo o
interno, sin costo. El uso comercial (venderlo, revenderlo, ofrecerlo
como parte de un servicio pago, etc.) requiere autorización escrita —
podés solicitarla a través de [midesmis.com](https://midesmis.com).

## Apoyá el proyecto

SureKey es gratis, sin publicidad, y sin ningún servicio de terceros
metido en el medio. Si te sirvió, una donación ayuda a que pueda seguir
dedicándole tiempo a este y a futuros proyectos igual de gratuitos.

**PayPal** → [paypal.me/arnulfoadg](https://paypal.me/arnulfoadg)

**Yappy** — escaneá este código QR desde la app:

<img src="donar-yappy-qr.png" alt="Código QR de Yappy para donar" width="280">

Invitarme un café me ayudaría a seguir creando más proyectos como este,
gratis. ¡Gracias!
