<img src="Pulso.ico" width="96" alt="Pulso icon">

# Pulso

> Formerly **xuca-monitor (Xuca Monitor)**: if you were searching for that name, this is the app.

Free app for **Windows 10 and 11** that does **all-round monitoring of your PC** from the tray (next to the clock): **network and latency**, **USB**, **gaming performance**, **Windows errors** and the **state of your CPU, graphics card, memory and disks**. When the internet drops, the ping spikes, something disconnects, FPS fall, the screen goes black, there's a blue screen or the hardware overheats, it tells you **what happened and what to try**. It also ships a **performance overlay** on top of the game and a **session recorder** that measures a few minutes of real use and builds a report with the bottleneck and what to do about it. On Windows 7 SP1 and 8.1 it runs in a reduced form (see [below](#windows-7-8-and-81)).

<!-- DEMO GIF: record it, save it as docs/demo.gif and delete the two comment markers on the next line -->
<!-- ![Demo](docs/demo.gif) -->

🇪🇸 [Versión en español más abajo](#-español) · Changelog: [English](CHANGELOG.en.md) · [Español](CHANGELOG.md)

## Download

**[Download Pulso.exe](https://github.com/011Xuca/pulso/releases/latest/download/Pulso.exe)**

Nothing to install: open it and go. It uses the .NET that Windows already ships (if yours is missing it, the app tells you and takes you to the .NET Framework 4.8 download).

> The first time, Windows may show a blue warning (SmartScreen) because the `.exe` isn't signed. Click **More info** and then **Run anyway**.

> **It asks for administrator rights on launch.** That's needed to read the real sensors (temperatures, watts, disks). If you say no to that prompt, the app doesn't open.
>
> **With nothing installed besides Pulso** you get real data for: the graphics card (temperature, hot spot, memory, clocks, usage, VRAM, power and fans, using the NVIDIA or AMD driver DLLs you already have), NVMe disks (temperature, life left, data written and hours, read straight from the disk), the SMART status of hard drives (Healthy / Caution / Bad, with reallocated, pending and uncorrectable sectors and CRC errors; an HDD has no life % and none is made up), the processor (usage, real frequency and performance % according to Windows, package and per-core **power** if Windows exposes the `Energy Meter` counter, and a **hint** of throttling if under high load Windows limits it or the frequency drops), RAM, paging and battery (% and watts on laptops). What can't be read without a driver is stated plainly: **"CPU temperature: requires driver (PawnIO, optional)"**, and the same for voltage (and power, on machines without that counter).
>
> **CPU temperature:** without drivers, Pulso uses the thermal zone Windows provides when it's credible (on laptops it usually works; on many desktops Windows gives no real value and nothing is shown). For the **real** CPU temperature and watts and the motherboard fans there is **PawnIO**, a free, signed, open-source driver (the same one LibreHardwareMonitor and FanControl use). It is **optional** and **Pulso doesn't download or install it**: the **How to install PawnIO** button on the CPU tab opens [pawnio.eu](https://pawnio.eu/) or copies the command `winget install --id namazso.PawnIO -e`. It's normal for the antivirus or SmartScreen to warn when installing a driver like this. Once it finishes installing, Pulso detects it on its own.
>
> **Antivirus:** up to 1.0.3 two things made Defender complain: the "Install PawnIO" button downloaded the driver installer and ran it from Pulso, and Pulso recreated its "Start with Windows" task (with `schtasks.exe` and a temporary XML) **every time it opened** (alerts `Behavior:Win32/Persistence.A!ml` and `Execution.A!ml`). Since 1.0.4 Pulso neither downloads nor runs any installer, and the task is handled through the Task Scheduler API and only touched when you turn it on or when you've moved the `.exe`. If Defender had already removed it, restore it from Windows Security > Virus & threat protection > Protection history.

## What it does

| Tab | What it's for |
|---|---|
| **Network** | At the top it shows **live latency**: ping to the internet, 10-second average, jitter, packet loss and ping to the router. It warns when the **internet drops** and tells you whether it was the network card (cable or WiFi), the router or your provider, and when the **ping rises and stays high (lag)**: how much the PC is downloading or uploading, which programs tend to use a lot of internet and whether a driver is leaking memory. |
| **USB** | When Windows plays the "something disconnected" sound, it tells you **which device it was** (mouse, keyboard, headset, monitor, Bluetooth...), whether it reconnected on its own and what to try. |
| **Performance** | While you play in fullscreen, it watches CPU, RAM, disk and graphics card. When something **lowers your FPS** it names the culprit: a background program (Windows Update, the antivirus, a launcher downloading...), a CPU throttling from heat, full RAM or a disk at 100%. It also warns if **another program steals focus from the game** without you touching anything (Discord, Steam, a Windows notification...). With the CPU tab on, it knows whether the processor throttles because of heat or power. |
| **Errors** | It warns you when the **screen goes black** because the video driver crashed, when there's a **blue screen** (with the code explained), when the PC **shuts off suddenly**, when there are hardware, disk or out-of-memory errors, and when a **game or program closes on its own** (and what failed: NVIDIA/AMD driver, an overlay...). With the hardware tabs on, it also tells you **how the PC was right before** and whether heat was the cause. |
| **CPU** | Temperature (against its TjMax limit), total and per-core usage, speed, **real** power and voltage, a chart of the last 5 minutes, and below it the **motherboard, BIOS, power plan, battery and fans**. It warns if the processor hits its limit and throttles (heat or power throttling), if its fan stops, if your Intel 13th/14th gen needs a new BIOS and if you game on the power-saver plan or on battery. |
| **GPU** | Core temperature, hot spot and memory, usage, clocks, power, fans, **VRAM** used and driver. It warns if it overheats, if the hot spot is far above the core (thermal paste), if the fans stop, if VRAM fills up or if the 12VHPWR connector is overloaded. On integrated graphics (Radeon Vega, Intel UHD) it shows shared memory and only the data that graphics chip reports. |
| **Memory** | RAM used, modules (brand, model, current and rated speed), whether **XMP/EXPO** is on, whether it runs in **single or dual channel**, and **paging** (pages per second read from disk and page file usage). It warns if RAM fills up (and what fills it) or if the modules overheat. |
| **Disks** | Each disk with its **type and bus (NVMe, SATA SSD, HDD, USB)**, its **SMART health** according to Windows and live **read and write speed**; and with sensors, temperature, life left, space used, data written and power-on hours. It warns if a disk throttles from heat, has little life left or Windows flags it as failing. |
| **Session** | **Session recorder**: pick 1, 5, 10, 15 or 30 minutes and use the PC or play normally (it isn't a synthetic benchmark). At the end it leaves a report with **average FPS, 1% and 0.1% lows, frametimes**, stutters, temperatures and CPU and GPU usage, **VRAM, RAM and paging**; the **bottleneck** (CPU vs GPU, VRAM, available RAM, disk type and resolution); a **system diagnosis** (full or trimmed-down Windows edition, recent critical errors with `sfc /scannow` and `DISM`, processes, services and autostart, days without a reboot, Windows 11 with 8 GB or less, memory reserved by integrated graphics, single channel and XMP, paging on an HDD) and **when it's worth using [Brío](https://github.com/011Xuca/brio)**, with a button to open or download it. |
| **Performance overlay** | On top of the game: FPS, session average, minimum and maximum, 1% and 0.1% lows, frametime with a chart, and usage, temperature, speed and power draw of CPU and GPU, VRAM and RAM. It's built from a text template and ships ready-made presets (including one for comparison videos). It injects nothing into the game: FPS come from Windows events (DirectX 9 to 12), like PresentMon. OBS captures it as the "Pulso Overlay" window. In its settings, **Categories to show** turns each part on or off without touching the template: FPS and 1% / 0.1% lows, Frametime and chart, CPU, GPU, VRAM, RAM, Temperatures, CPU and GPU names, and Time and game (remembered in `overlay.txt`). |

- Each tab can be **turned on or off** with the **Enabled** checkbox (or right-click on the tray icon). When off it costs nothing.
- It shows the network options from [Brío](https://github.com/011Xuca/brio) that you have applied, so you know whether any of them is related.
- It reads the Windows errors from that moment and shows them next to the alert.
- **It updates itself:** each time you open it, it checks GitHub once for a new version. If there is one, a yellow banner appears (and an entry in the tray menu); click it, it downloads from GitHub and reopens. Before replacing itself it checks that the download is complete, and if the new version fails to open it goes back to the previous one on its own.

## How to use it

1. Open `Pulso.exe`. It stays in the tray, next to the clock.
2. Tick **Start with Windows** so it launches on its own (it starts hidden in the tray).
3. Use your PC normally. When something happens, open the app from the tray and you'll see what it was.

- Minimizing or closing with the X leaves it in the tray. To close it completely: right-click the icon > **Exit**.
- The window never opens by itself: alerts are saved and you read them whenever you want.
- **Theme** light, dark or system, and **language** Spanish or English (the first time it uses Windows' language).
- **Zoom:** Ctrl + mouse wheel (or Ctrl + / Ctrl -) makes the text bigger or smaller. Ctrl + 0 resets it.

## Where it stores things

Everything lives in **Documents\Pulso**:

- `registro_*.txt` (network, usb, performance, errors, session, and `registro_pc_*` for CPU, GPU, Memory and Disks; `registro_pc.txt` gathers all the hardware): everything each tab detected, with timestamps (**Open log** button).
- `historial_*.txt`: what you see in the window (the last 300 alerts of each tab). **Clear** deletes it.
- `pc_ultimos_minutos.txt`: how the PC was in the last 5 minutes (useful to know whether a blackout was caused by temperature).
- `ajustes_monitores.txt`: theme, language, zoom and which tabs are on.
- `overlay.txt`: the performance overlay settings.
- `benchmarks\`: one `.txt` per recorded session (the same report you see in Session) and `benchmarks.csv`, with one row per session (`;`-separated) to compare them or open them in a spreadsheet.
- `crash.txt`: only if the app hit an error.

It sends nothing to any server. The only things that go out to the internet are the Network tab's ping (to the router, to 1.1.1.1 and to 8.8.8.8), the version check against GitHub and Pulso's update when you ask for it.

## Windows 7, 8 and 8.1

Pulso is built for **Windows 10 and 11**. On **Windows 7 SP1 and 8.1 it runs in a reduced form**:

- **Work:** Network, USB, Errors and Performance (the latter without graphics card and CPU speed data).
- **Missing:** the **CPU, GPU, Memory and Disks** tabs (temperatures, power draw, clocks and hardware state: the PawnIO driver is for Windows 10 and 11; a **Hardware** tab explaining this takes their place), the **performance overlay** and **session recording** (they use Windows 10 and 11 events). The app says so on those tabs and on the overlay button.
- **.NET Framework 4.8** is required: those Windows versions don't ship it, and if it's missing the app takes you to the download.
- **Windows 8 (not 8.1) isn't supported**: it can't install the .NET that's needed. The upgrade to 8.1 is free.

## Build it yourself

The code is in `src` (C# 5 for .NET Framework 4.8). Open `compilar.bat` and it builds `Pulso.exe` with the compiler Windows already ships. Nothing to install. The DLLs in the `lib` folder (sensors) are embedded **inside** the `.exe`: the result is a single file that works from any folder.

### Publish a release

1. Change `Version` in `src/App.cs` (and the one in `app.manifest`) and push the change.
2. Create a Release with the tag `v` + version (for example `v1.0.4`). The `.github/workflows/publicar.yml` workflow builds `Pulso.exe` and uploads it to the Release (the app looks for that file to update itself), builds `Pulso.msix` for the Microsoft Store and the winget manifests (`winget-manifests` artifact).
3. **winget:** the first time, submit the manifests by hand with `wingetcreate submit <winget-manifests folder>`. After that, with the `WINGET_TOKEN` secret, every Release is submitted automatically.
4. **Microsoft Store:** the first time, create the app in Partner Center (name "Pulso"), set the repo variables `MSIX_PUBLISHER` (the `CN=...` Partner Center shows) and `STORE_PRODUCT_ID`, and in the secrets the Entra ID app's data (`PARTNER_CENTER_TENANT_ID`, `PARTNER_CENTER_SELLER_ID`, `PARTNER_CENTER_CLIENT_ID`, `PARTNER_CENTER_CLIENT_SECRET`). The Store signs the package; when installed from there, Pulso doesn't update itself (the Store does).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Issues and pull requests are welcome, in English or Spanish. [MIT license](LICENSE).

## Credits

Made by **011Xuca**. Links: https://linktr.ee/011xuca
If it helped you, you can support me on Ko-fi: https://ko-fi.com/011xuca

Hardware temperatures and sensors use [LibreHardwareMonitorLib](https://github.com/LibreHardwareMonitor/LibreHardwareMonitor) (MPL-2.0 license).

---

## 🇪🇸 Español

> Antes se llamaba **xuca-monitor (Xuca Monitor)**: si lo buscabas con ese nombre, es esta app.

App gratis para **Windows 10 y 11** que hace el **monitoreo integral de tu PC** desde la bandeja (al lado del reloj): **red y latencia**, **USB**, **rendimiento en juegos**, **errores de Windows** y el **estado del procesador, la placa de video, la memoria y los discos**. Cuando se corta internet, sube el ping, se desconecta algo, bajan los FPS, la pantalla se pone negra, hay un pantallazo azul o el hardware se calienta de más, te dice **qué fue y qué probar**. Además trae un **overlay de rendimiento** encima del juego y un **grabador de sesión** que mide unos minutos de uso real y te arma un reporte con el cuello de botella y qué conviene hacer. En Windows 7 SP1 y 8.1 anda recortada (ver [más abajo](#windows-7-8-y-81)).

### Descargar

**[Descargar Pulso.exe](https://github.com/011Xuca/pulso/releases/latest/download/Pulso.exe)**

No hace falta instalar nada: lo abrís y listo. Usa el .NET que ya trae Windows (si a tu Windows le falta, la app te avisa y te lleva a la descarga de .NET Framework 4.8).

> La primera vez Windows puede mostrar un cartel azul (SmartScreen) porque el `.exe` no está firmado. Tocá **Más información** y después **Ejecutar de todas formas**.

> **Pide permisos de administrador al abrir.** Es necesario para leer los sensores reales (temperaturas, vatios, discos). Si le decís que no a ese permiso, la app no abre.
>
> **Sin instalar nada más que Pulso** se ven datos reales de: la placa de video (temperatura, hot spot, memoria, frecuencias, uso, VRAM, potencia y ventiladores, con las DLL del driver de NVIDIA o AMD que ya tenés), los discos NVMe (temperatura, vida útil, escrito y horas, pedidos directo al disco), el estado SMART de los discos rígidos (Sano / Atención / Mal, con sectores reasignados, pendientes, incorregibles y errores CRC; un HDD no tiene % de vida y no se inventa), el procesador (uso, frecuencia real y % de rendimiento según Windows, **potencia** del paquete y de cada núcleo si Windows expone el contador `Energy Meter`, y un **indicio** de throttling si con carga alta Windows lo limita o la frecuencia cae), la RAM, la paginación y la batería (% y vatios en notebooks). Lo que no se puede leer sin driver se dice tal cual: **"Temperatura de CPU: requiere driver (PawnIO, opcional)"**, y lo mismo para el voltaje (y la potencia, en los equipos sin ese contador).
>
> **Temperatura del procesador:** sin drivers, Pulso usa la zona térmica que da Windows cuando es creíble (en notebooks suele andar; en muchas PCs de escritorio Windows no da un valor real y no se muestra nada). Para la temperatura y los vatios **reales** del procesador y los ventiladores de la placa está **PawnIO**, un driver gratis, firmado y de código abierto (el mismo que usan LibreHardwareMonitor y FanControl). Es **opcional** y **Pulso no lo baja ni lo instala**: el botón **Cómo instalar PawnIO** de la pestaña CPU te abre [pawnio.eu](https://pawnio.eu/) o te copia el comando `winget install --id namazso.PawnIO -e`. Es normal que el antivirus o SmartScreen avisen al instalar un driver así. Cuando termina de instalarse, Pulso lo detecta solo.
>
> **Antivirus:** hasta la 1.0.3 había dos cosas que hacían saltar a Defender: el botón "Instalar PawnIO" bajaba el instalador del driver y lo ejecutaba desde Pulso, y Pulso volvía a crear su tarea de "Iniciar con Windows" (con `schtasks.exe` y un XML temporal) **cada vez que se abría** (alertas `Behavior:Win32/Persistence.A!ml` y `Execution.A!ml`). Desde la 1.0.4 Pulso no descarga ni ejecuta ningún instalador, y la tarea se maneja con la API del Programador de tareas y solo se toca cuando la prendés o cuando moviste el `.exe`. Si Defender ya la había borrado, restaurala desde Seguridad de Windows > Protección contra virus y amenazas > Historial de protección.

### Qué hace

| Pestaña | Para qué sirve |
|---|---|
| **Red** | Arriba muestra la **latencia en vivo**: ping a internet, promedio de los últimos 10 s, variación (jitter), paquetes perdidos y ping al router. Avisa cuando se **corta internet** y te dice si fue la placa de red (cable o WiFi), el router o tu proveedor, y cuando el **ping sube y se queda alto (lag)**: cuánto está bajando o subiendo la PC, qué programas suelen usar mucho internet y si algún driver está perdiendo memoria. |
| **USB** | Cuando suena el ruido de Windows de "se desconectó algo", te dice **qué aparato fue** (mouse, teclado, auriculares, monitor, Bluetooth...), si volvió a conectarse solo y qué conviene probar. |
| **Rendimiento** | Mientras jugás en pantalla completa, vigila procesador, RAM, disco y placa de video. Cuando algo **hace bajar los FPS** te dice el culpable: un programa de fondo (Windows Update, el antivirus, un launcher descargando...), el procesador que se frena por temperatura, la RAM llena o el disco al 100 %. También avisa si **otro programa le saca el foco al juego** sin que toques nada (Discord, Steam, un aviso de Windows...). Con la pestaña CPU prendida sabe si el procesador se frena por temperatura o por energía. |
| **Errores** | Te avisa cuando la **pantalla se pone negra** porque se colgó el driver de video, cuando hay un **pantallazo azul** (con el código explicado), cuando la PC **se apaga de golpe**, cuando hay errores de hardware, del disco o falta de memoria, y cuando un **juego o programa se cierra solo** (y en qué falló: driver de NVIDIA/AMD, un overlay...). Con las pestañas de hardware prendidas también dice **cómo estaba la PC justo antes** y si fue por calor o no. |
| **CPU** | Temperatura (contra su límite TjMax), uso total y por núcleo, velocidad, potencia y voltaje **reales**, gráfico de los últimos 5 minutos, y abajo la **placa madre, la BIOS, el plan de energía, la batería y los ventiladores**. Avisa si el procesador llega a su límite y se frena (throttling por calor o por energía), si su ventilador se para, si tu Intel 13.ª/14.ª gen necesita una BIOS nueva y si jugás con el plan de ahorro o a batería. |
| **GPU** | Temperatura del núcleo, hot spot y memoria, uso, frecuencias, potencia, ventiladores, **VRAM** usada y driver. Avisa si se calienta de más, si el hot spot queda muy separado del núcleo (pasta térmica), si los ventiladores se paran, si la VRAM se llena o si el conector 12VHPWR se sobrecarga. En gráficos integrados (Radeon Vega, Intel UHD) muestra la memoria compartida y solo los datos que esa gráfica informa. |
| **Memoria** | RAM usada, módulos (marca, modelo, velocidad actual y de fábrica), si el **XMP/EXPO** está activo, si anda en **single o dual channel**, y la **paginación** (páginas por segundo leídas del disco y uso del archivo de paginación). Avisa si la RAM se llena (y qué la llena) o si los módulos se calientan. |
| **Discos** | Cada disco con su **tipo y bus (NVMe, SSD SATA, HDD, USB)**, su **salud SMART** según Windows y la **velocidad de lectura y escritura** en vivo; y con sensores, temperatura, vida útil, espacio usado, datos escritos y horas encendido. Avisa si un disco se frena por temperatura, si le queda poca vida o si Windows lo marca con fallas. |
| **Sesión** | **Grabador de sesión**: elegís 1, 5, 10, 15 o 30 minutos y usás la PC o jugás normal (no es un benchmark sintético). Al terminar deja un reporte con **FPS promedio, 1 % y 0.1 % low, frametimes**, tirones, temperaturas y uso de CPU y GPU, **VRAM, RAM y paginación**; el **cuello de botella** (CPU contra GPU, VRAM, RAM disponible, tipo de disco y resolución); un **diagnóstico del sistema** (edición de Windows completa o recortada, errores críticos recientes con `sfc /scannow` y `DISM`, procesos, servicios e inicio automático, días sin reiniciar, Windows 11 con 8 GB o menos, memoria reservada por la gráfica integrada, single channel y XMP, paginación sobre HDD) y **cuándo conviene usar [Brío](https://github.com/011Xuca/brio)**, con un botón para abrirlo o descargarlo. |
| **Overlay de rendimiento** | Encima del juego: FPS, promedio, mínimo y máximo de la sesión, 1 % y 0.1 % low, frametime con gráfico, y uso, temperatura, velocidad y consumo de CPU y GPU, VRAM y RAM. Se arma con una plantilla de texto y trae modelos listos (incluido uno para videos comparativos). No inyecta nada en el juego: los FPS salen de los eventos de Windows (DirectX 9 a 12), como PresentMon. OBS lo captura como la ventana "Pulso Overlay". En sus ajustes, **Qué categorías mostrar** prende o apaga cada parte sin tocar la plantilla: FPS y 1 % / 0.1 % low, Frametime y gráfico, CPU, GPU, VRAM, RAM, Temperaturas, Nombres de CPU y GPU, y Hora y juego (se recuerda en `overlay.txt`). |

- Cada pestaña se puede **prender o apagar** con la casilla **Activado** (o con clic derecho en el ícono de la bandeja). Apagada no gasta nada.
- Muestra las opciones de red del [Brío](https://github.com/011Xuca/brio) que tenés aplicadas, así sabés si alguna tiene que ver.
- Lee los errores de Windows de ese momento y te los muestra junto al aviso.
- **Se actualiza sola:** cada vez que la abrís se fija una vez en GitHub si hay una versión nueva. Si hay, aparece un cartel amarillo (y una opción en el menú de la bandeja); lo tocás, se baja de GitHub y se vuelve a abrir. Antes de reemplazarse verifica que la descarga esté completa, y si la versión nueva no llega a abrir vuelve sola a la anterior.

### Cómo se usa

1. Abrí `Pulso.exe`. Queda en la bandeja, al lado del reloj.
2. Marcá **Iniciar con Windows** para que arranque solo (arranca escondido en la bandeja).
3. Usá la PC normal. Cuando pase algo, abrí la app desde la bandeja y vas a ver qué pasó.

- Minimizar o cerrar con la X la deja en la bandeja. Para cerrarla del todo: clic derecho en el ícono > **Salir**.
- La ventana nunca se abre sola: los avisos se guardan y los ves cuando quieras.
- **Tema** claro, oscuro o el del sistema, e **idioma** español o inglés (la primera vez usa el de Windows).
- **Zoom:** Ctrl + rueda del mouse (o Ctrl + / Ctrl -) agranda o achica el texto. Ctrl + 0 lo vuelve al tamaño normal.

### Dónde guarda las cosas

Todo queda en **Documentos\Pulso**:

- `registro_*.txt` (red, usb, rendimiento, errores, sesion y `registro_pc_*` para CPU, GPU, Memoria y Discos; `registro_pc.txt` junta todo el hardware): todo lo que detectó cada pestaña, con hora (botón **Abrir registro**).
- `historial_*.txt`: lo que ves en la ventana (los últimos 300 avisos de cada pestaña). **Limpiar** lo borra.
- `pc_ultimos_minutos.txt`: cómo estuvo la PC en los últimos 5 minutos (sirve para saber si un apagón fue por temperatura).
- `ajustes_monitores.txt`: tema, idioma, zoom y qué pestañas están prendidas.
- `overlay.txt`: los ajustes del overlay de rendimiento.
- `benchmarks\`: un `.txt` por sesión grabada (el mismo reporte que ves en Sesión) y `benchmarks.csv`, con una fila por sesión (separado por `;`) para compararlas o abrirlas en una planilla.
- `crash.txt`: solo si la app tuvo un error.

No manda nada a ningún servidor. Lo único que sale a internet es el ping de la pestaña Red (al router, a 1.1.1.1 y a 8.8.8.8), la consulta de versión a GitHub y la actualización de Pulso cuando la pedís.

### Windows 7, 8 y 8.1

Pulso está hecha para **Windows 10 y 11**. En **Windows 7 SP1 y 8.1 anda recortada**:

- **Andan:** Red, USB, Errores y Rendimiento (este último sin los datos de la placa de video ni de la velocidad del procesador).
- **No están:** las pestañas **CPU, GPU, Memoria y Discos** (temperaturas, consumo, frecuencias y estado del hardware: el driver PawnIO es para Windows 10 y 11; en su lugar queda una pestaña **Hardware** que lo explica), el **overlay de rendimiento** y la **grabación de sesión** (usan eventos de Windows 10 y 11). La app lo avisa en esas pestañas y en el botón del overlay.
- Hace falta **.NET Framework 4.8**: esos Windows no lo traen, y si falta la app te lleva a la descarga.
- **Windows 8 (sin el .1) no es compatible**: no puede instalar el .NET que hace falta. La actualización a 8.1 es gratis.

### Compilarlo vos

El código está en `src` (C# 5 para .NET Framework 4.8). Abrí `compilar.bat` y te arma `Pulso.exe` con el compilador que ya trae Windows. No hace falta instalar nada. Las dll de la carpeta `lib` (sensores) se meten **adentro** del `.exe`: el resultado es un solo archivo que anda en cualquier carpeta.

### Publicar una versión

1. Cambiá `Version` en `src/App.cs` (y la de `app.manifest`) y subí el cambio.
2. Creá un Release con el tag `v` + versión (por ejemplo `v1.0.4`). El workflow `.github/workflows/publicar.yml` compila `Pulso.exe` y lo sube al Release (la app busca ese archivo para actualizarse sola), arma `Pulso.msix` para la Microsoft Store y los manifiestos de winget (artefacto `winget-manifests`).
3. **winget:** la primera vez enviá los manifiestos a mano con `wingetcreate submit <carpeta winget-manifests>`. Después, con el secreto `WINGET_TOKEN`, cada Release se envía solo.
4. **Microsoft Store:** la primera vez creá la app en Partner Center (nombre "Pulso"), cargá en las variables del repo `MSIX_PUBLISHER` (el `CN=...` que muestra Partner Center) y `STORE_PRODUCT_ID`, y en los secretos los datos de la app de Entra ID (`PARTNER_CENTER_TENANT_ID`, `PARTNER_CENTER_SELLER_ID`, `PARTNER_CENTER_CLIENT_ID`, `PARTNER_CENTER_CLIENT_SECRET`). La Store firma el paquete; instalada desde ahí, Pulso no se actualiza sola (lo hace la Store).

### Colaborar

Mirá [CONTRIBUTING.md](CONTRIBUTING.md). Se aceptan issues y pull requests, en español o en inglés. [Licencia MIT](LICENSE).

### Créditos

Hecho por **011Xuca**. Redes: https://linktr.ee/011xuca
Si te sirvió, podés apoyarme en Ko-fi: https://ko-fi.com/011xuca

Las temperaturas y sensores de hardware usan [LibreHardwareMonitorLib](https://github.com/LibreHardwareMonitor/LibreHardwareMonitor) (licencia MPL-2.0).
