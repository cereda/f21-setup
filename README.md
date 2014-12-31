# My Fedora 21 setup

*Upgrade everything:*

```bash
$ sudo dnf upgrade
```

*Add RPM Fusion repositories:*

```bash
$ wget http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-21.noarch.rpm
$ wget http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-21.noarch.rpm
$ sudo dnf install rpmfusion-free-release-21.noarch.rpm rpmfusion-nonfree-release-21.noarch.rpm
```

*Add the Moka repository and install icon theme:*

```bash
$ wget http://mokaproject.com/packages/rpm/moka-stable.repo
$ sudo mv moka-stable.repo /etc/yum.repos.d/
$ sudo dnf install faba-icon-theme faba-mono-icons
```

*Install Google Chrome and Voice and Video (repository is automatically added):*

```bash
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
$ wget http://dl.google.com/linux/direct/google-talkplugin_current_x86_64.rpm
$ sudo dnf install google-chrome-stable_current_x86_64.rpm google-talkplugin_current_x86_64.rpm
```

*Install the Adobe repository:*

```bash
$ wget http://linuxdownload.adobe.com/adobe-release/adobe-release-i386-1.0-1.noarch.rpm
$ sudo dnf install adobe-release-i386-1.0-1.noarch.rpm
```

*Install the Flash Player plugin:*

```bash
$ sudo dnf install flash-plugin
```

*Add colours to the terminal:*

Create a file named `colours.sh` with the following content:

```bash
if [[ ! -z \$BASH ]]; then
    if [[ \$USER = "root" ]]; then
        PS1="\[\033[33m\][\[\033[m\]\[\033[31m\]\u@\h\[\033[m\] \[\033[33m\]\W\[\033[m\]\[\033[33m\]]\[\033[m\] # "
    elif [[ $(whoami) = "root" ]]; then
        PS1="\[\033[33m\][\[\033[m\]\[\033[31m\]\u@\h\[\033[m\] \[\033[33m\]\W\[\033[m\]\[\033[33m\]]\[\033[m\] # "
    else
        PS1="\[\033[36m\][\[\033[m\]\[\033[34m\]\u@\h\[\033[m\] \[\033[32m\]\W\[\033[m\]\[\033[36m\]]\[\033[m\] \$ "
    fi
fi
```

And move it to `/etc/profile.d/`:

```bash
$ sudo mv colours.sh /etc/profile.d/
```

*Enable systemwide touchpad tap:*

Create a file named `00-enable-taps.conf` with the following content:

```bash
Section "InputClass"
	Identifier "tap-by-default"
	MatchIsTouchpad "on"
	Option "TapButton1" "1"
EndSection
```

And move it to `/etc/X11/xorg.conf.d/`:

```bash
$ sudo mv 00-enable-taps.conf /etc/X11/xorg.conf.d/
```

*Improve font rendering:*

First things first:

```bash
$ sudo dnf install freetype-freeworld
```

Then create a file named `local.conf` with the following content:

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
	<match target="pattern">
		<edit name="dpi" mode="assign">96</edit>
	</match>
	<match target="font">
		<edit mode="assign" name="antialias" >
			<bool>true</bool>
		</edit>
	</match>
	<match target="font">
		<edit mode="assign" name="hinting" >
			<bool>true</bool>
		</edit>
	</match>
	<match target="font">
		<edit mode="assign" name="hintstyle" >
			<const>hintslight</const>
		</edit>
	</match>
	<match target="font">
		<edit mode="assign" name="rgba" >
			<const>rgb</const>
		</edit>
	</match>
	<match target="font">
		<edit mode="assign" name="lcdfilter">
			<const>lcddefault</const>
		</edit>
	</match>
</fontconfig>
```

And move it to `/etc/fonts/`:

```bash
$ sudo mv local.conf /etc/fonts/
```

*Add media codecs:*

```bash
$ sudo dnf install amrnb amrwb faac faad2 flac gstreamer1-libav gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly gstreamer-ffmpeg gstreamer-plugins-bad-nonfree gstreamer-plugins-espeak gstreamer-plugins-fc gstreamer-plugins-ugly gstreamer-rtsp lame libdca libmad libmatroska x264 xvidcore gstreamer1-plugins-bad-free gstreamer1-plugins-base gstreamer1-plugins-good gstreamer-plugins-bad gstreamer-plugins-bad-free gstreamer-plugins-base gstreamer-plugins-good
```

*Add some useful softwares:*

```bash
$ sudo dnf install cabextract lzip nano p7zip p7zip-plugins unrar
```

*Configure spf13-vim*:

First things first:

```bash
$ sudo dnf install vim-enhanced vim-X11
```

Then install `spf13-vim`:

```bash
$ curl http://j.mp/spf13-vim3 -L -o - | sh
```

Install `powerline`:

```bash
$ sudo dnf install powerline
```

Install Powerline fonts from `https://github.com/powerline/fonts`:

```bash
$ wget https://github.com/powerline/fonts/archive/master.zip
$ unzip master.zip
$ cd fonts-master/
$ ./install.sh
$ sudo fc-cache -fsv ~/.fonts
```

Edit `~/.vimrc.before` and uncomment/edit:

```bash
let g:airline_powerline_fonts = 1
let g:spf13_noninvasive_completion = 1
let g:spf13_no_big_font = 1
```

And add the following content to `~/.vimrc.local`:

```bash
set guifont=Droid\ Sans\ Mono\ for\ Powerline\ 11
```

*Install GTK themes and icons (and the proper GNOME extension):*

```bash
$ sudo dnf install faba-icon-theme.noarch moka-icon-theme.noarch echo-icon-theme.noarch egtk-gtk2-theme.noarch egtk-gtk3-theme.noarch mono-icon-theme.noarch gnome-icon-theme.noarch orchis-gtk-theme.noarch plank-theme-moka.noarch light-gtk2-theme.noarch light-gtk3-theme.noarch tango-icon-theme.noarch fedora-icon-theme.noarch light-theme-gnome.noarch nimbus-icon-theme.noarch nuvola-icon-theme.noarch oxygen-icon-theme.noarch rodent-icon-theme.noarch zukini-gtk2-theme.noarch zukini-gtk3-theme.noarch zukiwi-gtk2-theme.noarch zukiwi-gtk3-theme.noarch adwaita-gtk2-theme.x86_64 adwaita-icon-theme.noarch hicolor-icon-theme.noarch plank-theme-orchis.noarch faience-icon-theme.noarch nimbus-theme-gnome.noarch nodoka-theme-gnome.noarch zukitwo-gtk2-theme.noarch zukitwo-gtk3-theme.noarch bluebird-gtk2-theme.noarch bluebird-gtk3-theme.noarch greybird-gtk2-theme.noarch greybird-gtk3-theme.noarch humanity-icon-theme.noarch adwaita-cursor-theme.noarch albatross-gtk2-theme.noarch albatross-gtk3-theme.noarch bluecurve-gtk-themes.x86_64 bluecurve-icon-theme.noarch oxygen-cursor-themes.noarch bluecurve-gnome-theme.noarch monochrome-icon-theme.noarch moka-gnome-shell-theme.noarch bluecurve-cursor-theme.noarch gnome-theme-curvylooks.noarch gnome-colors-icon-theme.noarch gnome-icon-theme-extras.noarch tango-icon-theme-extras.noarch gnome-shell-theme-selene.noarch gnome-shell-theme-zukiwi.noarch gnome-icon-theme-symbolic.noarch gnome-shell-theme-zukitwo.noarch clearlooks-phenix-gtk2-theme.noarch clearlooks-phenix-gtk3-theme.noarch gnome-shell-extension-user-theme.noarch

```

*Fortune cookies in the terminal:*

First things first:

```bash
$ sudo dnf install fortune-mod
```

Then add the following content to `~/.bashrc`:

```bash
if [ -f /usr/bin/fortune ]; then
	/usr/bin/fortune
	echo
fi
```

*Configure TeX Live:*

Create a symbolic link:

```bash
$ sudo ln -s /usr/local/texlive/<year>/bin/<arch> /opt/texbin
```

Create a file named `texlive.sh` with the following content:

```bash
#!/bin/bash
pathmunge () {
	if ! echo $PATH | /bin/egrep -q "(^|:)$1($|:)" ; then
		if [ "$2" = "after" ] ; then
			PATH=$PATH:$1
		else
			PATH=$1:$PATH
		fi
	fi
}
pathmunge /opt/texbin
unset pathmunge
```

Then move it to `/etc/profile.d/`:

```bash
$ sudo mv texlive.sh /etc/profile.d/
```

Then configure OpenType fonts from TeX Live:

```bash
$ sudo cp $(kpsewhich -var-value TEXMFSYSVAR)/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.conf
$ sudo fc-cache -fsv
```

*Make vim map default Windows shortcuts:*

And add the following content to `~/.vimrc.local`:

```bash
source $VIMRUNTIME/mswin.vim
behave mswin
```

*Initial Git configuration:*

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
$ git config --global core.editor vim
$ git config --global push.default simple
```

To be continued.

