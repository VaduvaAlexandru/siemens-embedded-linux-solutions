QEMU_SYSTEM = qemu-system-arm
KERNEL = kernel-qemu-4.19.50-buster
DTB = versatile-pb-buster.dtb
MACHINE = versatilepb
CPU = arm1176
ROOTFS = 2021-05-07-raspios-buster-armhf-full.img
NIC_MODEL = smc91c111
BRIDGE = virbr0
ETH = ens33
BRIDGE_ID = "qemu_bridge"
ARGS = "root=/dev/sda2 console=ttyAMA0"

extract:
	sudo apt install xz-utils
	wget https://downloads.raspberrypi.org/raspios_full_armhf/images/raspios_full_armhf-2021-05-28/2021-05-07-raspios-buster-armhf-full.zip
	unzip 2021-05-07-raspios-buster-armhf-full.zip
	wget https://github.com/dhruvvyas90/qemu-rpi-kernel/raw/master/kernel-qemu-4.19.50-buster
	wget https://github.com/dhruvvyas90/qemu-rpi-kernel/raw/master/versatile-pb-buster.dtb

bridge: extract
	sudo brctl addbr virbr0
	sudo brctl addif virbr0 $(ETH)
	sudo ip address flush dev $(ETH)
	sudo dhclient virbr0
	sudo mkdir /etc/qemu
	echo "allow virbr0" | sudo tee -a /etc/qemu/bridge.conf

run: 
	sudo $(QEMU_SYSTEM) \
	-kernel $(KERNEL) \
	-dtb $(DTB) \
	-M $(MACHINE) \
	-cpu $(CPU) \
	-drive file=$(ROOTFS),index=0,media=disk,format=raw \
	-append $(ARGS) \
	-net nic,model=$(NIC_MODEL),netdev=$(BRIDGE_ID) \
	-netdev bridge,br=$(BRIDGE),id=$(BRIDGE_ID) \
	-serial stdio

clean:
	rm -rf $(KERNEL) $(DTB) $(ROOTFS) 2021-05-07-raspios-buster-armhf-full.zip
