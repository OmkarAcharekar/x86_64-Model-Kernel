# Build Setup Modek to Compile a Kernel for `x86_64`

A compatible  binary (kernel) which refers to a specification that defines the handoff from a bootloader to a payload. The `ELF64-x86_64`-file contains the binary   that can be loaded  by a GRUB bootloader. It fetches data from `UEFI` firmware and `cpuid` and logs that information to the screen. The main goals were to figure out how such a setup will look like

The demo project focuses on the **x86_64** processor architecture  and `UEFI` as firmware environment.

![Rust Kernel QEMU Screenshot](./doc/figures/rust-kernel-qemu-screenshot.png "Rust Kernel QEMU Screenshot")
*Screenshot from our kernel running in QEMU. It can fetch some information about it's environment
and print it to the screen. If you boot it on your private computer, it would look similar.*


## Build Process of the Kernel
The assembly code will do some minimal setup of the stack and other things, before it jumps into the code generated by the Rust compiler. `cargo` assembles The final binary. Also include global assembly and uses a custom linker script.


## How To Run

When you test this project (`run_qemu.sh`), it will
1) start QEMU + loads `edk2/OVMF` as `UEFI`-environment 
2) `OVMF` will automatically boot `GRUB` (an EFI file)
3) the `GRUB`-EFI-file has a `grub.cfg` file and the Rust binary built-in into the `GRUB`-internal `(memdisk)`-filesystem.
4) `GRUB` loads the cfg-file which starts the binary


With a boot-order of `firmware > GRUB > %my-binary%`, the binary could take the role of:
* another bootloader (multistage boot)
* an OS-Kernel written in Rust 
* an OS-specific loader which prepares hand-off to an OS-kernel (to decouple large software into smaller blocks of responsibility)

---

### Software/Tool versions 
#### Required Packages & Tools
- `rustc` and `cargo` (e.g. via `rustup`)
- `gcc` for global assembly files
- `ovmf` (for testing in `QEMU` only)
#### Notes
- I tested the built on an Ubuntu 20.04 system with Linux 5.8.0
  - probably doesn't build on other operating systems than Linux (not tested)
- because of the file`rust-toolchain.toml` `cargo` will use the correct compiler
  version by default
- "run in QEMU"-demonstration tested with
  - **GRUB**: 2.04-1ubuntu26.1
  - **QEMU**: 4.2.1
  - `OVMF` files are searched in `/usr/share/OVMF`

---



