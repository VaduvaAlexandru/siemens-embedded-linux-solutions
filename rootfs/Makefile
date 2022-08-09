QEMU_SYSTEM = qemu-system-arm
KERNEL = ../qemu/run_pi_qemu/kernel-qemu-4.19.50-buster
DTB = ../qemu/run_pi_qemu/versatile-pb-buster.dtb
MACHINE = versatilepb
CPU = arm1176
ROOTFS = rootfs_image
NIC_MODEL = smc91c111
BRIDGE = virbr0
BRIDGE_ID = "qemu_bridge"
ARGS = "root=/dev/sda2 console=ttyAMA0"

.PHONY: run clean

run: $(ROOTFS)
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

$(ROOTFS):
	sudo ./make_rootfs.sh

clean:
	rm -f $(ROOTFS)
