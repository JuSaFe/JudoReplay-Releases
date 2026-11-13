# Componentes de terceros

Bibliotecas y programas que JudoReplay incorpora y bajo qué licencia. Todos son compatibles con la
distribución de este proyecto en los términos de su [licencia](LICENSE) — ninguno obliga a publicar
el programa como código abierto ni a permitir obras derivadas.

**Ninguna impone condiciones de uso.** A diferencia de JudoAdministración, aquí no entra QuestPDF
(la única del otro proyecto con licencia dual): JudoReplay no genera PDF.

Todas son permisivas (MIT, y SIL OFL para la tipografía) salvo **FFmpeg**, que va con licencia
**LGPL 2.1 o posterior**: el ejecutable como programa aparte y las bibliotecas enlazadas de forma
dinámica, todo sustituible (ver §2).

---

## 1. Paquetes NuGet

### Aplicación de escritorio (`JudoReplay.csproj`)

| Paquete | Versión | Licencia | Notas |
|---|---|---|---|
| Avalonia, Avalonia.Desktop, Avalonia.Themes.Fluent, Avalonia.Diagnostics | 11.3.6 | MIT | `Avalonia.Diagnostics` se excluye en Release |
| Avalonia.Fonts.Inter | 11.3.6 | MIT (paquete) · **SIL OFL 1.1** (tipografía Inter) | Redistribución libre; hay que conservar el aviso de copyright de la fuente |
| CommunityToolkit.Mvvm | 8.4.0 | MIT | |
| FFmpeg.AutoGen | 8.1.0 | MIT | El reproductor: las declaraciones para llamar a las bibliotecas de FFmpeg, que carga en tiempo de ejecución |
| Microsoft.Extensions.DependencyInjection | 10.0.3 | MIT | |
| ASP.NET Core (Kestrel), marco compartido `Microsoft.AspNetCore.App` | 9.0 | MIT | El servidor de vídeo del CARE en el puerto 8450 (`Services/Replay/ServidorVideo.cs`). Va dentro del publicado autónomo |

### Cliente de la API (`JudoReplay.Client`) y contratos (`JudoReplay.Shared`)

Sin dependencias externas: solo la biblioteca base de .NET (`HttpClient`, `ClientWebSocket`,
`System.Text.Json`).

## 2. Plataforma y herramientas

| Componente | Licencia | Cómo se usa |
|---|---|---|
| .NET 9 | MIT | Runtime incluido en los paquetes autocontenidos |
| **FFmpeg** 8.1.3 | **LGPL 2.1 o posterior** | Graba con el **ejecutable** `ffmpeg`, que JudoReplay lanza como proceso hijo, y reproduce con las **bibliotecas compartidas** (avcodec, avformat, avutil, swscale, swresample), cargadas en tiempo de ejecución. Todo va en la carpeta `ffmpeg/` junto al ejecutable (Windows y Linux) |

### FFmpeg

- **Qué compilación:** la LGPL de BtbN, `FFmpeg-Builds`, fijada por nombre y suma SHA-256 en
  [`Empaquetado/ffmpeg/version.env`](Empaquetado/ffmpeg/version.env). Origen de los binarios:
  <https://github.com/BtbN/FFmpeg-Builds/releases>. Código fuente de FFmpeg: <https://ffmpeg.org/download.html>
  y <https://git.ffmpeg.org/ffmpeg.git> (la versión exacta va en `ffmpeg -version`). Los guiones de
  compilación de BtbN, para rehacerla: <https://github.com/BtbN/FFmpeg-Builds>.
- **Su licencia** va dentro del paquete, en `ffmpeg/LICENSE.txt`.
- **Se puede sustituir:** cualquiera puede poner otra compilación de FFmpeg 8.1 en esa carpeta (o señalarlas
  con las variables de entorno `JUDO_FFMPEG` y `JUDO_FFMPEG_LIB`) sin tocar JudoReplay, que es lo que
  pide la LGPL: el enlace con las bibliotecas es dinámico.
- **No es GPL:** la compilación no incluye x264 ni x265. Trae algunos codificadores por software con
  licencias permisivas (SVT-AV1, libaom, rav1e, kvazaar), que JudoReplay no usa: solo codifica por
  hardware (Documentación/01-Plan.md, D2).
- **macOS:** todavía sin FFmpeg dentro del `.dmg`, porque no hay compilación LGPL publicada para Mac.
  Hasta compilar una propia, la aplicación usa el FFmpeg del sistema.
- **Patentes:** H.265 está sujeto a patentes. La aplicación es gratuita y codifica con el hardware del
  propio equipo, sin distribuir un codificador por software (Documentación/01-Plan.md, §4.2).

## 3. Marcas y logotipos

Los archivos de `Assets/Images` (escudo de la Federación de Judo de la Comunidad Valenciana) son
propiedad de sus respectivas entidades. La licencia de este proyecto cubre el código, **no** esas
marcas: su uso requiere permiso de sus titulares.

Los iconos de `Assets/Icons` y los dibujos vectoriales de `Styles/Icons.axaml` son obra del titular
del proyecto y se rigen por la [licencia](LICENSE) general.

## 4. Relación con JudoAdministración

JudoReplay **no** enlaza con ningún ensamblado de JudoAdministración: los dos repositorios son
independientes. Lo que comparten son los contratos de la API —las clases de `JudoReplay.Shared`, que
son una copia de las del servidor— y eso es código del mismo titular, bajo la misma licencia. Ver
[Documentación/01-Plan.md](Documentación/01-Plan.md), §3.2.
