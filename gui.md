---
title: Рабочий стол на Arch Linux
date: '2022-03-17'
tags: ['arch', 'nvidia', 'aur', 'btrfs']
draft: false
summary: Подготовим Arch в качестве десктопа для работы и игорь.
images: []
---

<TOCInline toc={props.toc} asDisclosure />

---

Привет! В этой статье мы подготовим наш [свежий Arch](https://www.kittan.ru/blog/archinstall) к работе в качестве домашнего десктопа.  

Мы используем:  

- рабочий стол `KDE Plasma` или `GNOME`
- проприетарный DKMS драйвер `NVIDIA`  
- `yay` (майонез)  
- `zramd` в качестве SWAP
- `timeshift` для резервного копирования  

---

## Подготовка  

- #### Включим 32-битный репозиторий  

Нам нужны будут некоторые зависимости оттуда.  
Для этого в `/etc/pacman.conf` раскомментируем репозиторий multilib:  

```
[multilib]
Include=/etc/pacman.d/mirrorlist
```  

Если хотите получить доступ к тестовой версии Plasma, то в начале списка репозиториев (перед записью [testing]) в `/etc/pacman.conf` добавьте:  

```
[kde-unstable]
Include = /etc/pacman.d/mirrorlist
```  

А если к тестовой версии Gnome, то:  

```
[gnome-unstable]
Include = /etc/pacman.d/mirrorlist
```  

Раз мы зашли сюда, раскомментируем еще и `Color` c `ParallelDownloads = 5` в разделе `Misc options`

```
# Misc options
#UseSyslog
Color
#NoProgressBar
CheckSpace
#VerbosePkgLists
ParallelDownloads = 5
```
Это включит параллельную загрузку и подсветку в терминале.

---  

- #### Обновим систему  

```
sudo pacman -Suy
```

---  

## Установка среды рабочего стола  

Сейчас я использую GNOME и KDE Plasma, поэтому опишу их установку.  

На мой взгляд, основные отличия Gnome от Plasma:  

- Gnome полностью построен на GTK, а Plasma на Qt
- В дизайне интерфейса для Gnome на первом месте пользовательский опыт, а для Plasma - функциональность
- Gnome использует новый графический сервер Wayland по умолчанию и неплохо с ним работает. У KDE с Wayland всё еще могут [быть](https://community.kde.org/KWin/Wayland) какие-то проблемы. А могут и [не быть](https://www.phoronix.com/scan.php?page=news_item&px=Qt-Wayland-NVIDIA-Thread), лучше проверьте работу своей системы на Plasma сперва с Wayland.  
 
---  

- #### Установим Gnome и включим GDM 

GNOME использует [Wayland](https://wiki.archlinux.org/title/wayland) по умолчанию.  


```
sudo pacman -S gnome gnome-extra
```
```
sudo systemctl enable gdm
```

---

Список установленных пакетов:  

| Пакет   | Описание |
|:-----------|:--|
|[gdm](https://wiki.archlinux.org/title/GDM)|менеджер дисплея Gnome|
|[gnome](https://archlinux.org/groups/x86_64/gnome/)|группа пакетов с десктопом и основными приложениями|
|[gnome-extra](https://archlinux.org/groups/x86_64/gnome-extra/)|группа пакетов с дополнительными приложениями|  

---  

- #### Или установим KDE Plasma и включим SDDM

KDE установим с Xorg.  

```
sudo pacman -S xorg-server xorg-apps plasma kde-applications
```
```
sudo systemctl enable sddm
```

---  

Список установленных пакетов:  

| Пакет   | Описание |
|:-----------|--|
|[xorg-server](https://wiki.archlinux.org/title/Xorg#Installation)|Xorg сервер|
|[xorg-apps](https://archlinux.org/groups/x86_64/xorg-apps/)|группа пакетов с конфигами для Xorg|
|[sddm](https://wiki.archlinux.org/title/SDDM)|менеджер дисплея KDE|
|[plasma](https://archlinux.org/groups/x86_64/plasma/)|группа пакетов с десктопом и основными приложениями|
|[kde-applications](https://archlinux.org/groups/x86_64/kde-applications/)|группа пакетов с группами дополнительных приложениями|

Вместо kde-application можно выбрать только нужные вам группы [тут](https://archlinux.org/packages/extra/any/kde-applications-meta/) или [тут](https://archlinux.org/packages/kde-unstable/any/kde-applications-meta/)  
 
---  

## Установка проприетарного драйвера NVIDIA  

Установим проприетарный DKMS драйвер NVIDIA. Иногда запускаю игры и мне нужна производительность. А DKMS позволит нам не возиться с модулями ядра при обновлении.  

- #### Установим драйвер NVIDIA и Vulkan  

```
sudo pacman -S nvidia-dkms nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader opencl-nvidia lib32-opencl-nvidia libxnvctrl
```
Список установленных пакетов:  

| Пакет   | Описание |
|:-----------|--|
|[nvidia-dkms](https://archlinux.org/packages/extra/x86_64/nvidia-dkms/)|[DKMS](https://wiki.archlinux.org/title/Dynamic_Kernel_Module_Support) проприетарный драйвер NVIDIA|
|[nvidia-utils](https://archlinux.org/packages/extra/x86_64/nvidia-utils/)|утилиты драйвера NVIDIA|
|[lib32-nvidia-utils](https://archlinux.org/packages/multilib/x86_64/lib32-nvidia-utils/)|утилиты драйвера NVIDIA (32-bit)|
|[nvidia-settings](https://wiki.archlinux.org/title/NVIDIA#nvidia-settings)|редактор опций NVIDIA|
|[vulkan-icd-loader](https://wiki.archlinux.org/title/Vulkan)|графический API Vulkan|
|[lib32-vulkan-icd-loader](https://archlinux.org/packages/multilib/x86_64/lib32-vulkan-icd-loader/)|графический API Vulkan (32-bit)|
|[opencl-nvidia](https://wiki.archlinux.org/title/GPGPU#OpenCL)|среда выполнения OpenCL для NVIDIA|
|[lib32-opencl-nvidia](https://archlinux.org/packages/multilib/x86_64/lib32-opencl-nvidia/)|среда выполнения OpenCL для NVIDIA (32-bit)|
|[libxnvctrl](https://archlinux.org/packages/extra/x86_64/libxnvctrl/)|API для NVIDIA и X|

---  

- #### Включим модули ядра для NVIDIA и btrfs

Отредактируем скрипт Initial ramdisk `/etc/mkinitcpio.conf`.  
В строку `MODULES` добавим `nvidia nvidia_modeset nvidia_uvm nvidia_drm crc32c libcrc32c zlib_deflate btrfs`.  

Теперь пересоберем образ ядра:  
```
sudo mkinitcpio -P
```
Обновим загрузчик:  
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
```
reboot
```  

---

- #### Настроим драйвер NVIDIA  

Сперва сгенерируем файл конфигурации X сервера:  

```
sudo nvidia-xconfig
```
```
reboot
```

Теперь запустим настройки NVIDIA  

```
sudo nvidia-settings
```

Во вкладке `X Server XVideo Settings` выберем основной монитор.  
Во вкладке `PowerMizer` в разделе `PowerMizer Settings` выберем `Prefer Maximum Performance`.  
Во вкладке `X Server Display Configuration` выберем наше разрешение и частоту и сохраним `Save to X Configuration File`.  
Запустим `nvidia-settings` без `sudo` и повторим всё настройки выше. Но не будем сохранять через `Save to X Configuration File`.  

---  


## Начальная настройка  

- #### Установим yay  

Одно из главных преимуществ Arch Linux - это Arch User Repository (AUR). В пользовательских репозиториях очень быстро появляются новые версии пакетов.  

Скрипты с информацией о сборке пакетов тут неофициальные. У меня никогда не возникало проблем и я ничего не слышал о взломах через AUR, но будьте благоразумны.  

[yay](https://github.com/Jguer/yay) - один из помощников AUR. С его помощью можно устанавливать и обновлять пакеты из AUR и обычных репозиториев.  

Использование без ключей выполнит поиск пакета, содержащего искомые слова в названии или описании. Поиск идет по подключенным репозиториям и в AUR.  

Клонируем репозиторий с yay и установим его с помощью [makepkg](https://wiki.archlinux.org/title/Makepkg):  

```
git clone https://aur.archlinux.org/yay.git ~/Downloads/yay
```
```
cd ~/Downloads/yay
```
```
makepkg -sric
```
`-i` - установит пакет после сборки  
`-s` - установит недостающие зависимости  
`-r` - удалит зависимости для сборки после ее окончания  
`-c` - очистит каталог установки  

---  

- #### Настроим файл подкачки  

`zramd` - служба, создающая файл подкачки в памяти. Я использую её вместо файла подкачки.

Установим и запустим службу `zramd`:

```
yay zramd
```
```
sudo systemctl enable --now zramd
```

Вывод `lsblk` покажет SWAP

---  

- #### Настроим резервное копирование  



---  

Ура! Наша система почти готова. Но уже сейчас ей можно пользоваться не боясь прострелить себе .

В [следующей статье](https://www.kittan.ru/blog/improve) мы оптимизируем и украсим нашу систему. 

Надеюсь, эта статья была полезна вам! А если у вас возникла проблема, вы можете рассказать о ней в комментариях. Я обязательно отвечу.

