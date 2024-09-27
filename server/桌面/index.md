安装 gnome-tweaks 工具

```
dnf install gnome-tweaks
```

Dock

```bash
dnf install \
  gnome-extensions-app \
  gnome-shell-extension-dash-to-dock
```

主题资源的网址

- <https://www.gnome-look.org/browse>

- <https://www.gnome-look.org/p/1403328/>
- <https://github.com/vinceliuice/WhiteSur-gtk-theme>

```
dnf install -y glib2-devel sassc
```

把图标、光标，放到 `/usr/share/icons` 文件夹下；将主题放入 `/usr/share/themes` 目录

打开配置 GUI

```bash
$ gnome-tweaks

# 设置 icon fonts
```

插件配置 GUI

```bash
$ gnome-extensions-app

# 设置主题
```

中文输入法

```
dnf install ibus-libpinyin.x86_64  ibus -y
```

