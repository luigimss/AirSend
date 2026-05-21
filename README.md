# AirSend 
<p align="center">
  <a href="README.md">English</a> ·
  <a href="README.es.md">Español</a>
</p>

Desktop app for Windows that captures the system audio and send it to a HomePod through AirPlay 2 with low latency

> ⚠️ **State**: pre-alfa.



## Instalation Guide
Install the latest version in:

<a href="https://github.com/Pabldi08/AirSend/releases/latest">
  <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download latest release">
</a>


> SmartScreen warning: As the app is not signed with an EV code certificate (approximately €200/year), the first run will display a warning. Workaround for the user: "Learn more" → "Run anyway". Consider EV signing in post-MVP.

## Comparison with alternatives

Esta tabla resume las diferencias principales entre AirSend y algunas alternativas populares.  
AirSend prioriza ser gratuito, auditable y compatible con HomePod mediante AirPlay 2, aunque todavía es un proyecto joven y no pretende cubrir funciones como mirroring de pantalla o multi-destino.

| Característica | AirSend | TuneBlade | AirParrot |
|---|---|---|---|
| Plataforma | Windows | Windows | Windows + macOS |
| Licencia / precio | GPL-2.0, gratis | Freeware, closed-source | Comercial (~15 €) |
| AirPlay 2 con HomePod | ✅  | ⚠️ Inestable / no pairea en muchos casos | ✅ |
| Pair-setup transient + verify | ✅ | ❌, sólo AirPlay 1 fiable | ✅ |
| Codec | ALAC + ChaCha20-Poly1305 | ALAC | ALAC |
| Captura | WASAPI loopback, audio del sistema | WASAPI loopback | WASAPI loopback + cherry-pick de apps |
| Tray + close-to-tray | ✅ | ✅ | ✅ |
| Auto-reconnect al último device | ✅ | ✅ | ✅ |
| IP manual si mDNS falla | ✅ | ❌ | ⚠️ Limitado |
| Latencia HomePod estable | 500 ms – 3 s configurable | Variable | Alta, desync ocasional |
| Jitter Windows en release | ✅ MMCSS Pro Audio + HIGH_PRIORITY | ❌ | ✅ |
| Desarrollo activo | ✅ | ❌, parado desde hace años | ✅ |
| Código auditable | ✅ | ❌ | ❌ |
| Probado en hardware | ✅ | ✅ | ✅ |

## Estado del proyecto

AirSend está en una fase inicial de desarrollo. La versión actual ya permite enviar audio del sistema desde Windows a un HomePod mediante AirPlay 2, pero todavía no pretende sustituir completamente a soluciones comerciales más maduras.

El objetivo principal del proyecto es ofrecer una alternativa gratuita, abierta y auditable para usuarios que quieran enviar audio desde Windows a dispositivos AirPlay compatibles.

## Roadmap

### Versión actual: 0.1.4

- [x] Captura de audio del sistema mediante WASAPI loopback
- [x] Envío de audio a dispositivos AirPlay 2
- [x] Soporte para HomePod mediante pair-setup
- [x] Reconexión automática al último dispositivo
- [x] Opción de IP manual cuando mDNS falla
- [x] Integración con bandeja del sistema
- [x] Auto-update firmado con minisign

### Próximas mejoras

- [ ] Mejorar el sistema de descubrimiento de dispositivos por mDNS
- [ ] Añadir selector gráfico de dispositivo
- [ ] Mejorar mensajes de error y logs
- [ ] Reducir avisos de SmartScreen mediante firma de código
- [ ] Mejorar la estabilidad en redes con VLANs o routers problemáticos
- [ ] Añadir documentación técnica sobre AirPlay 2 y pairing

### Futuro

- [ ] Soporte multi-dispositivo
- [ ] Selección de audio por aplicación
- [ ] Perfiles de latencia
- [ ] Mejor integración con Windows
- [ ] Tests automatizados de red y audio

## Publicar una release

1. Bump la versión en `src-tauri/tauri.conf.json` (`version`) y en `Cargo.toml` (`workspace.package.version`).
2. Commit y tag: `git tag v0.1.0 && git push origin v0.1.0`.
3. GitHub Actions (`.github/workflows/release.yml`) compila en `windows-latest`, firma con la clave de actualización (secrets `TAURI_SIGNING_PRIVATE_KEY` y `TAURI_SIGNING_PRIVATE_KEY_PASSWORD`) y crea un **release en draft** con el `.exe` de NSIS, `.exe.sig` y `latest.json`.
4. Cuando el draft esté listo y validado, **publícalo a mano** desde GitHub. Las apps instaladas detectarán el update.

## Stack

- **Rust** + **Tauri v2** (UI con webview nativo de Windows, sin Electron)
- **mdns-sd** para descubrir dispositivos AirPlay en la red local
- **WASAPI loopback** para capturar el audio del sistema (sólo Windows)
- **ALAC** + **RTP cifrado** para enviar al HomePod (pendiente)
- Instalador **NSIS** y auto-updater vía **GitHub Releases**

## Licencia

GPL-2.0 (compatible con `airplay2-rs`, usado como referencia).
