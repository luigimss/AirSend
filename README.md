# ConexionAirPlay

App de escritorio para Windows que captura el audio del sistema y lo envía a un HomePod por AirPlay 2.

> ⚠️ **Estado**: pre-alfa. Hito 1 del plan (scaffold + discovery mDNS) en curso.

## Stack

- **Rust** + **Tauri v2** (UI con webview nativo de Windows, sin Electron)
- **mdns-sd** para descubrir dispositivos AirPlay en la red local
- **WASAPI loopback** para capturar el audio del sistema (sólo Windows)
- **ALAC** + **RTP cifrado** para enviar al HomePod (pendiente)
- Instalador **NSIS** y auto-updater vía **GitHub Releases**

## Layout

```
crates/airplay-core   # protocolo: discovery + (pair / RTSP / RTP en próximos hitos)
crates/audio-capture  # WASAPI loopback (stub en Linux/macOS)
crates/audio-encode   # PCM → ALAC (placeholder)
src-tauri             # app Tauri (lib + main + config + capabilities)
ui                    # frontend Vite + TS (vanilla, minimalista)
```

## Desarrollo

Requisitos: Rust ≥ 1.78, Node ≥ 20, npm. En Linux para dev local; para `tauri build` final se necesita Windows (o cross-compile).

```bash
# instalar deps frontend
npm --prefix ui install

# arrancar en modo dev (compila Rust + abre webview)
cargo install tauri-cli --locked --version "^2"   # solo la primera vez
cargo tauri dev
```

En Linux verás la ventana con la lista de dispositivos AirPlay detectados (HomePod, Apple TV, etc.). La captura de audio sólo funcionará en Windows.

## Build Windows

```bash
cargo tauri build --target x86_64-pc-windows-msvc
# produce src-tauri/target/x86_64-pc-windows-msvc/release/bundle/nsis/*.exe
```

## Plan completo

`/home/pablo/.claude/plans/quiero-tener-la-posibilidad-majestic-tower.md`

## Publicar una release

1. Bump la versión en `src-tauri/tauri.conf.json` (`version`) y en `Cargo.toml` (`workspace.package.version`).
2. Commit y tag: `git tag v0.1.0 && git push origin v0.1.0`.
3. GitHub Actions (`.github/workflows/release.yml`) compila en `windows-latest`, firma con la clave de actualización (secrets `TAURI_SIGNING_PRIVATE_KEY` y `TAURI_SIGNING_PRIVATE_KEY_PASSWORD`) y crea un **release en draft** con el `.exe` de NSIS, `.exe.sig` y `latest.json`.
4. Cuando el draft esté listo y validado, **publícalo a mano** desde GitHub. Las apps instaladas detectarán el update.

> Aviso de SmartScreen: al no firmar con certificado EV de código (≈200 €/año), la primera ejecución mostrará un aviso. Workaround para el usuario: "Más información" → "Ejecutar igualmente". Considerar firma EV en post-MVP.

## Licencia

GPL-2.0 (compatible con `airplay2-rs`, usado como referencia).
