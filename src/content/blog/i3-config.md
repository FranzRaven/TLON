---
title: 'i3 config'
description: ''
pubDate: 'Jan 07 2022'

---

### Install & config

	sudo xbps-install i3-gaps i3-status dbus polkit elogind
Luego habilitar dbus y polkitd (ln -s /etc/sv/ /var/service/)

### Poner lo siguiente en /etc/polkit-1/rules.d/10-udisks.rules
	polkit.addRule(function(action, subject) {
    var permitted_actions = [
        "org.freedesktop.udisks2.filesystem-mount",
        "org.freedesktop.udisks2.encrypted-unlock",
        "org.freedesktop.udisks2.eject-media",
        "org.freedesktop.udisks2.power-off-drive",
        "org.freedesktop.udisks2.filesystem-mount-system",
        "org.freedesktop.udisks2.encrypted-unlock-system"
    ];

    if (subject.isInGroup("wheel") && ~permitted_actions.indexOf(action.id)) {
	return polkit.Result.YES
    }
	});

### Poner lo siguiente en .bashrc 
	# Ensure XDG_RUNTIME_DIR is set
	if test -z "$XDG_RUNTIME_DIR"; then
	    export XDG_RUNTIME_DIR=$(mktemp -d /tmp/$(id -u)-runtime-dir.XXX)
	fi

### Poner lo siguiente en .xinitrc
	setxkbmap latam  &
	pipewire &
	nitrogen --restore &
	picom --config /home/$USER/.config/picom.conf &


	exec dbus-launch --exit-with-session  i3


### Pipewire
Configurar como indica la página de Void Linux.  
Lanzar pipewire en .xinitrc o en i3/config

### NVIDIA & Cuda
Instalar repos non free de Void

	sudo xbps-install void-repo-nonfree
	
Instalar drivers de los repos de Void, luego ...
	
	wget --> nvidia drivers de pag oficial Nvidia para ubuntu
	sudo sh *.run --silent --toolkit --samples --samplespath=/home/	user/cuda-test/
	Ejemplo -->(wget https://developer.download.nvidia.com/compute/cuda/13.0.1/local_installers/cuda_13.0.1_580.82.07_linux.run)

### i3status.conf
Descargar [i3status.conf](../../public/i3status.conf)

### i3 config
Descargar i3 [config](../../public/config)

### Broadcom 4312
	git clone https://github.com/void-linux/void-packages
	cd void-packages
	./xbps-src binary-bootstrap
	echo XBPS_ALLOW_RESTRICTED=yes >> etc/conf
	./xbps-src pkg b43-firmware # for 6.30.163.46, b43-firmware-classic for 5.100.138
	sudo xbps-install -R hostdir/binpkgs/nonfree b43-firmware


### Weird home permissions
	sudo chmod -R a+rwX,o-w /home/$USER
	
### Browser fonts
	# FONTS
	sudo xbps-install -Rs noto-fonts-emoji noto-fonts-ttf noto-fonts-ttf-extra

	#FOnts config Borwser	
	sudo ln -s /usr/share/fontconfig/conf.avail/70-no-bitmaps.conf /etc/fonts/conf.d/
	sudo xbps-reconfigure -f fontconfig
