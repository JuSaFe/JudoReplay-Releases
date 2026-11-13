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
| **FFmpeg** 8.1.3 | **LGPL 2.1 o posterior** | Graba con el **ejecutable** `ffmpeg`, que JudoReplay lanza como proceso hijo, y reproduce con las **bibliotecas compartidas** (avcodec, avformat, avutil, swscale, swresample), cargadas en tiempo de ejecución. Todo va en la carpeta `ffmpeg/` junto al ejecutable |
| dav1d 1.5.4 | BSD 2 cláusulas | Dentro de las bibliotecas de FFmpeg: decodifica AV1 en los equipos sin AV1 por hardware |
| libvpl 2.16.0 | MIT | Dentro de las bibliotecas de FFmpeg (Windows y Linux): Intel QuickSync |
| nv-codec-headers 12.1.14.1, cabeceras AMF 1.5.0 | MIT | Solo cabeceras, para compilar FFmpeg con NVENC y AMF; los controladores se cargan del sistema |

### FFmpeg

- **Qué compilación:** una propia, la misma en Windows, macOS y Linux, hecha con
  [`Empaquetado/ffmpeg/compilar.sh`](Empaquetado/ffmpeg/compilar.sh) a partir del código de FFmpeg
  sin modificar: bibliotecas compartidas, **sin componentes GPL ni «nonfree»** y **sin codificadores
  de vídeo por software** (solo los de hardware de cada sistema). Ver
  [`Empaquetado/ffmpeg/LEEME.md`](Empaquetado/ffmpeg/LEEME.md).
- **El código fuente**, con el guion que la compila y las sumas, está publicado junto a los binarios
  en la release `ffmpeg-8.1.3-jr1` del repositorio público:
  <https://github.com/JuSaFe/JudoReplay-Releases/releases/tag/ffmpeg-8.1.3-jr1>. También en
  <https://ffmpeg.org/download.html>.
- **Dentro del paquete:** `ffmpeg/LEEME.txt` (qué es y dónde está su código), las licencias
  (`LICENCIA-*.txt`) y la orden `configure` exacta (`configuracion.txt`).
- **Se puede sustituir:** cualquiera puede poner otra compilación de FFmpeg 8.1 en esa carpeta (o
  señalarla con las variables de entorno `JUDO_FFMPEG` y `JUDO_FFMPEG_LIB`) sin tocar JudoReplay,
  que es lo que pide la LGPL: el enlace con las bibliotecas es dinámico.
- **Patentes:** H.265 está sujeto a patentes. La aplicación es gratuita y codifica con el hardware del
  propio equipo, sin distribuir un codificador de vídeo por software (Documentación/01-Plan.md, §4.2).

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
