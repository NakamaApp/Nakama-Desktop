# Nakama

Приложение для просмотра аниме на компьютере — для **Windows** и **Linux**.

- большой каталог с поиском и фильтрами по жанрам, годам, статусу;
- выбор озвучки и плеера (Kodik, Alloha, CVH, Aniboom, Sibnet), выбор качества;
- удобный плеер: перемотка, скорость, полноэкранный режим, пропуск опенинга, автопереход на следующую серию;
- избранное, история, отметки просмотренных серий и продолжение с того же места;
- синхронизация избранного и прогресса с Nakama на телевизоре (через Google Drive);
- приложение само сообщает о выходе новой версии.

## Скачать

Откройте последний [релиз](https://github.com/NakamaApp/Nakama-Desktop/releases/latest) и скачайте файл для своей системы:

| Система | Файл |
| --- | --- |
| Windows 10 / 11 (64-bit) | `Nakama.msi` |
| Ubuntu, Debian, Linux Mint, Pop!_OS | `nakama-1_amd64.deb` |
| Fedora, openSUSE | `nakama-1.x86_64.rpm` |

Нужен 64-битный компьютер (x86_64).

## Windows

1. Запустите `Nakama.msi` и следуйте подсказкам установщика.
2. Если Windows покажет «Windows защитила ваш компьютер», нажмите **Подробнее → Выполнить в любом случае**
   (у установщика нет платной цифровой подписи).
3. Запускайте Nakama из меню «Пуск» или с ярлыка на рабочем столе.

Всё нужное уже внутри установщика. Если в системе нет компонента Microsoft Edge WebView2 (нужен для
плеера Alloha), установщик поставит его сам.

Новая версия ставится поверх старой — просто запустите новый `.msi`. Удаление: «Параметры → Приложения →
Nakama → Удалить».

## Linux

Пакет ставится обычным пакетным менеджером — он сам доустановит всё нужное (VLC, WebKitGTK,
GStreamer и системные библиотеки).

### Ubuntu / Debian / Linux Mint / Pop!_OS

```bash
sudo apt install ./nakama-1_amd64.deb
```

Кодеки в этих системах полные из коробки — больше ничего делать не нужно.

### Fedora

```bash
sudo dnf install ./nakama-1.x86_64.rpm
```

**Обязательно для Fedora:** стандартные кодеки Fedora не умеют показывать большинство видео (будет звук
без картинки). Один раз подключите репозиторий RPM Fusion и поставьте полные кодеки:

```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf install vlc-plugins-freeworld
```

### openSUSE

```bash
sudo zypper install ./nakama-1.x86_64.rpm
```

Полные кодеки — из репозитория Packman (для Leap замените `openSUSE_Tumbleweed` на свою версию,
например `openSUSE_Leap_15.6`):

```bash
sudo zypper addrepo -cfp 90 https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman
sudo zypper dup --from packman --allow-vendor-change
```

### Запуск и удаление

Nakama появится в меню приложений (раздел «Аудио и видео»). Из терминала: `/opt/nakama/bin/Nakama`.

Обновление — так же, как установка: поставьте новый пакет поверх старого. Удаление:

```bash
sudo apt remove nakama      # Ubuntu / Debian / Mint
sudo dnf remove nakama      # Fedora
sudo zypper remove nakama   # openSUSE
```

### Если пакет ставили вручную

Если пакет поставлен в обход пакетного менеджера (`dpkg -i`, `rpm -i`) и что-то не работает,
доустановите зависимости сами:

```bash
# Ubuntu / Debian / Mint
sudo apt install vlc curl xdg-utils libwebkit2gtk-4.1-0 \
  gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-libav

# Fedora (плюс полные кодеки из раздела «Fedora» выше)
sudo dnf install vlc curl xdg-utils webkit2gtk4.1 gstreamer1-plugins-good gstreamer1-plugin-openh264

# openSUSE
sudo zypper install vlc curl xdg-utils libwebkit2gtk-4_1-0 gstreamer-plugins-good gstreamer-plugins-libav
```

## Если что-то не работает

- **Звук есть, картинки нет (Linux)** — не установлены полные кодеки, см. Fedora / openSUSE выше.
- **Не открывается серия** — попробуйте другую озвучку или плеер: источники иногда временно недоступны.
- **Каталог не загружается** — сайты-источники в некоторых регионах доступны только через VPN.
- **Нет плеера Alloha в Linux** — не установлен WebKitGTK (`libwebkit2gtk-4.1-0` / `webkit2gtk4.1`).
- **Alloha в Linux показывает чёрный экран** — проверьте, что стоит
  `gstreamer1.0-libav` (Ubuntu) или `gstreamer1-plugin-openh264` (Fedora).

Если проблема осталась, приложите к сообщению о ней файл журнала:
- Windows: `C:\Users\<имя>\.nakama\logs\nakama.log`
- Linux: `~/.nakama/logs/nakama.log`

Ваши данные (избранное, история, настройки) хранятся в папке `.nakama` в домашнем каталоге и не
удаляются вместе с приложением.
