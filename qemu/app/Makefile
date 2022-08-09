CC = arm-linux-gnueabihf-gcc
CFLAGS = -static
CPU = arm1176
QEMU_USER = qemu-arm

.PHONY: build clean

build: hello-app

hello-app: hello-world.c
	$(CC) $(CFLAGS) $^ -o $@

run: hello-app
	$(QEMU_USER) -cpu $(CPU) $<

clean:
	rm -f hello-app
