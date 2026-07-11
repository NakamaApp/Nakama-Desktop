# Nakama

**Nakama** — приложение для просмотра аниме на Windows.

Смотрите аниме в удобном плеере прямо на компьютере: большой каталог,
поиск по названию и жанрам, выбор озвучки и качества, полноэкранный режим.

## Возможности

- 📺 Большой каталог аниме с поиском и фильтрами
- 🎙️ Выбор озвучки и качества видео
- ⏯️ Удобный плеер: перемотка, скорость воспроизведения, полноэкранный режим
- ⭐ Избранное и история просмотра
- ✅ Отметки о просмотренных сериях и продолжение с того же места
- 🔄 Автопереход к следующей серии

## Установка

Скачайте установщик `Nakama-1.0.0.msi`, запустите его и следуйте подсказкам.
Всё необходимое уже внутри — дополнительно ничего ставить не нужно.


# Nakama для Linux — установка

Приложение для просмотра аниме. Пакеты самодостаточные: внутри уже есть Java-рантайм
и встроенный Chromium (обход Cloudflare). От системы нужен только VLC (ставится
автоматически как зависимость) и `curl` (есть во всех дистрибутивах из коробки).

Поддерживается только архитектура **x86_64 (amd64)**.

## Файлы

| Файл | Дистрибутивы |
| --- | --- |
| `nakama-1.0.0-1.x86_64.rpm` | Fedora, RHEL, AlmaLinux, Rocky, openSUSE |
| `nakama_1.0.0-1_amd64.deb` | Ubuntu, Debian, Linux Mint, Pop!_OS |

## Установка на Fedora / RHEL / openSUSE (.rpm)

```bash
sudo dnf install -y ./nakama-1.0.0-1.x86_64.rpm
```

(на openSUSE — `sudo zypper install ./nakama-1.0.0-1.x86_64.rpm`)

**Обязательно для Fedora:** штатный `ffmpeg-free` не умеет декодировать H.264 —
будет звук без картинки. Один раз подключите RPM Fusion и замените ffmpeg —
команды в разделе «Зависимости» ниже.

## Установка на Ubuntu / Debian / Mint (.deb)

```bash
sudo apt install ./nakama_1.0.0-1_amd64.deb
```

Кодеки на Ubuntu полные из коробки — дополнительных действий не нужно.
Если VLC не подтянулся автоматически: `sudo apt install vlc`.

## Запуск

- Из меню приложений: **Nakama** (категория «Аудио и видео»),
- или из терминала: `/opt/nakama/bin/Nakama`.

Первый запуск дольше обычного: приложение распаковывает браузерный движок
в `~/.nakama/kcef-bundle` (~440 МБ). Дальше запуск быстрый.

## Зависимости

Пакетный менеджер ставит зависимости автоматически, но если ставили пакет в обход
него (`rpm -i` / `dpkg -i`) или что-то пошло не так — вот полный список того, что
нужно приложению от системы, и как это доустановить вручную.

| Зависимость | Зачем нужна | Как проверить |
| --- | --- | --- |
| **VLC** (libvlc) | Нативное воспроизведение видео | `vlc --version` |
| **ffmpeg** (полный, с H.264) | Декодирование видео внутри VLC | `ffmpeg -version` |
| **curl** | Загрузка потоков некоторых плееров | `curl --version` |
| **xdg-utils** | Открытие внешних ссылок в браузере | `xdg-open --version` |

### Fedora / RHEL / AlmaLinux / Rocky

```bash
sudo dnf install -y vlc curl xdg-utils

# Полноценный ffmpeg — только из RPM Fusion (штатный ffmpeg-free не декодирует H.264):
sudo dnf install -y \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf swap ffmpeg-free ffmpeg --allowerasing -y
```

На RHEL/AlmaLinux/Rocky вместо первой ссылки RPM Fusion используйте вариант для EL:
`https://mirrors.rpmfusion.org/free/el/rpmfusion-free-release-$(rpm -E %rhel).noarch.rpm`
(предварительно нужен репозиторий EPEL: `sudo dnf install -y epel-release`).

### Ubuntu / Debian / Linux Mint

```bash
sudo apt update
sudo apt install -y vlc curl xdg-utils
```

Кодеки здесь полные из коробки (пакет `ffmpeg` при желании: `sudo apt install -y ffmpeg`).

### openSUSE

```bash
sudo zypper install vlc curl xdg-utils

# Полные кодеки — из репозитория Packman:
sudo zypper addrepo -cfp 90 https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman
sudo zypper dup --from packman --allow-vendor-change
```

(для Leap замените `openSUSE_Tumbleweed` на свою версию, например `openSUSE_Leap_15.6`)

### Другие дистрибутивы (Arch и т.п.)

Пакеты .rpm/.deb туда не встанут, но если запускаете распакованный app-image —
достаточно `vlc`, `ffmpeg`, `curl`, `xdg-utils` из штатного репозитория
(например, `sudo pacman -S vlc ffmpeg curl xdg-utils`).

## Удаление

```bash
# Fedora / RHEL / openSUSE
sudo dnf remove nakama

# Ubuntu / Debian / Mint
sudo apt remove nakama
```

Пользовательские данные (история, избранное, кэш браузера) остаются в `~/.nakama` —
при желании удалите каталог вручную: `rm -rf ~/.nakama`.

## Если что-то не работает

- **Звук есть, картинки нет** — не установлены кодеки H.264 (см. раздел про RPM Fusion выше).
- **Плеер не запускается** — проверьте, что установлен VLC: `vlc --version`.
- **Каталог не грузится** — сайты-источники могут требовать VPN в зависимости от региона.
- Для диагностики запустите из терминала (`/opt/nakama/bin/Nakama`) — логи выводятся в консоль.




## Чтобы использовать весь функционал требуется настроить маршрутизацию в ускорителе интернета*
В прямые подключения добавить:

domain:animego.me

domain:kodik.info

domain:cdn.kodik.info

domain:cloud.kodik.info

domain:trailers.kodik.info

domain:sibnet.ru

domain:dns.sibnet.ru

domain:ns.sinor.ru

domain:solodcdn.com

domain:kodik-storage.com

domain:cloud.kodik-storage.com

domain:kodik-cdn.com

domain:ls.player-cname-domain.com

domain:aniqit.com

domain:kodikplayer.com

domain:ya-ligh.com

domain:aniboom.one

suffix:.kodik-storage.com

suffix:.kodik-cdn.com

suffix:.solodcdn.com

suffix:.ru

suffix:.рф
