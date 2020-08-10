# Building an OS in Rust

I am following:

    https://os.phil-opp.com/

# Requirements

This code uses nightly rust:

    rustup override set nightly

We need the bootimage tool to link the kernel and the bootloader:

    rustup component add llvm-tools-preview

Now run the following command outside this directory (or it will inherit our weird cargo overrides and fail to build):

    cargo install bootimage

# Building

Compile a freestanding binary with:

    # Linux
    cargo rustc -- -C link-arg=-nostartfiles
    # Windows
    cargo rustc -- -C link-args="/ENTRY:_start /SUBSYSTEM:console"
    # macOS
    cargo rustc -- -C link-args="-e __start -static -nostartfiles"

But we have also overridden this in cargo so you should be able to just:

    cargo build

To build and link the kernel with a bootloader:

    cargo bootimage

Now we can run it in qemu with:

    qemu-system-x86_64 -drive format=raw,file=target/x86_64-os-in-rust/debug/bootimage-os-in-rust.bin 

But also we have overridden `cargo run` to do this so you can just do that instead.
