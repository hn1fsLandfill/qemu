# Compiling the emulator

### Note: for this you'll need the following dependencies (with their headers):
* GTK
* pixman
* slirp
* pulseaudio

### And these too (for the build system):
* ninja
* python3 (with pip and venv support; you probably have it installed)

For Arch: `pacman -S base-devel gtk3 libslirp pulseaudio ninja linux-api-headers python`

```sh
./configure --target-list="i386-softmmu"  --without-default-features --enable-pixman --enable-gtk --enable-slirp --enable-kvm --disable-fdt --enable-pa --enable-vpc
ninja -C build qemu-system-i386 -j$(nproc)
```

This will take around 10 minutes depending on your machine.

# Running

Obtain the 7z from https://archive.org/details/windows-phone-10-emulator-10.0.14393 and extract a file called "flash.vhd" from it

Finally in the QEMU directory run the following:
```sh
build/qemu-system-i386 \
    -nodefaults \
    -vga std \
    -m 1G \
    -cpu host \
    -smp cores=2 \
    -accel kvm \
    -usb \
    -device usb-tablet \
    -audiodev pa,id=pa \
    -device usb-audio,audiodev=pa \
    -hda <path to your flash.vhd> \
    -nic model=e1000
```