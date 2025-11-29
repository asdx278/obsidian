---
class: note
area: professional
tags:
  - linux
created: 2025-11-18
---
**MOC: [[MOC - Professional]]**

# Linux - Переименование стандартных папок

> [!tldr] ai review
> 

После установки, в зависимости от выбранного языка, стандартные папки будут названы на этом языке, например: Рабочий стол, Загрузки, Документы..., что не удобно.

Сопоставление папок и системных путей находится в файле 
```bash
cat ~/.config/user-dirs.dirs
```

Нужно сделать так для именования папок на английском:
```bash
XDG_DESKTOP_DIR="$HOME/Desktop"  
XDG_DOWNLOAD_DIR="$HOME/Downloads"  
XDG_TEMPLATES_DIR="$HOME/Templates"  
XDG_PUBLICSHARE_DIR="$HOME/Public"  
XDG_DOCUMENTS_DIR="$HOME/Documents"  
XDG_MUSIC_DIR="$HOME/Music"  
XDG_PICTURES_DIR="$HOME/Pictures"  
XDG_VIDEOS_DIR="$HOME/Videos"
```

Локаль находится тут:
```bash
cat ~/.config/user-dirs.locale
```

Также нужно в графическом интерфейсе обновить ссылки на папки

![[Pasted image 20251119000757.png]]

### Additional materials

https://www.linux.org.ru/forum/general/9224736

https://alt-gnome.wiki/tips/change-the-language-of-the-home-user-folders-automatically/

https://docs.fedoraproject.org/en-US/fedora/f40/system-administrators-guide/basic-system-configuration/System_Locale_and_Keyboard_Configuration/#System_Locale_and_Keyboard_Configuration.adoc#tab-locale_options