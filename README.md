# My Fedora 21 setup

Upgrade everything:

```bash
$ sudo dnf upgrade
```

Add RPM Fusion repositories:

```bash
$ wget http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-21.noarch.rpm
$ wget http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-21.noarch.rpm
$ sudo dnf install rpmfusion-free-release-21.noarch.rpm rpmfusion-nonfree-release-21.noarch.rpm
```

Add the Moka repository and install icon theme:

```bash
$ wget http://mokaproject.com/packages/rpm/moka-stable.repo
$ sudo mv moka-stable.repo /etc/yum.repos.d/
$ sudo dnf install faba-icon-theme faba-mono-icons
```

Install Google Chrome and Voice and Video (repository is automatically added):

```bash
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
$ wget http://dl.google.com/linux/direct/google-talkplugin_current_x86_64.rpm
$ sudo dnf install google-chrome-stable_current_x86_64.rpm google-talkplugin_current_x86_64.rpm
```

Install the Adobe repository:

```bash
$ wget http://linuxdownload.adobe.com/adobe-release/adobe-release-i386-1.0-1.noarch.rpm
$ sudo dnf install adobe-release-i386-1.0-1.noarch.rpm
```

Install the Flash Player plugin:

```bash
$ sudo dnf install flash-plugin
```

Add colours to the terminal: create a file named `colours.sh` with the following content:

```bash
if [[ ! -z \$BASH ]]; then
	if [[ \$USER = "root" ]]; then
		PS1="\[\033[33m\][\[\033[m\]\[\033[31m\]\u@\h\[\033[m\] \[\033[33m\]\W\[\033[m\]\[\033[33m\]]\[\033[m\] \$ "
	else
		PS1="\[\033[36m\][\[\033[m\]\[\033[34m\]\u@\h\[\033[m\] \[\033[32m\]\W\[\033[m\]\[\033[36m\]]\[\033[m\] \$ "
	fi
fi
```

and move it to `/etc/profile.d/`:

```bash
$ sudo mv colours.sh /etc/profile.d/
```

Enable systemwide touchpad tap:

Create a file named `00-enable-taps.conf` with the following content:

```bash
Section "InputClass"
	Identifier "tap-by-default"
	MatchIsTouchpad "on"
	Option "TapButton1" "1"
EndSection
```

and move it to `/etc/X11/xorg.conf.d/`:

```bash
$ sudo mv 00-enable-taps.conf /etc/X11/xorg.conf.d/
```

Improve font rendering:

First of all:

```bash
$ sudo dnf install freetype-freeworld
```

then create a file named `local.conf` with the following content:

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

and move it to `/etc/fonts/`:

```bash
$ sudo mv local.conf /etc/fonts/
```

Add media codecs:

```bash
$ sudo dnf install amrnb amrwb faac faad2 flac gstreamer1-libav gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly gstreamer-ffmpeg gstreamer-plugins-bad-nonfree gstreamer-plugins-espeak gstreamer-plugins-fc gstreamer-plugins-ugly gstreamer-rtsp lame libdca libmad libmatroska x264 xvidcore gstreamer1-plugins-bad-free gstreamer1-plugins-base gstreamer1-plugins-good gstreamer-plugins-bad gstreamer-plugins-bad-free gstreamer-plugins-base gstreamer-plugins-good
```

Add some useful softwares:

```bash
$ sudo dnf install cabextract lzip nano p7zip p7zip-plugins unrar
```

To be continued.

