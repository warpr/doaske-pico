Doaske Pico
===========

Generated from https://github.com/rp-rs/rp2040-project-template


Setup
=====

Download the [rustup](https://rustup.rs/) installer from https://sh.rustup.rs.

Then get some additional stuff to be able to compile and link a Raspberry Pico, and
create a .UF2 file which can be copied to the pico bootsel volume.

    rustup target install thumbv6m-none-eabi
    cargo install flip-link
    cargo install elf2uf2-rs


Build and run
=============

    cargo build
    cargo build [--release]

ELF binaries from the build are stored here:

    ./target/thumbv6m-none-eabi/debug/doaske
    ./target/thumbv6m-none-eabi/release/doaske

These ELF binaries can be converted to UF2 files using elf2uf2-rs, like this:

    elf2uf2-rs ./target/thumbv6m-none-eabi/release/doaske ./doaske.uf2

Alternatively, use `elf2uf2-rs -d` as the runner in .cargo/config.toml, which
allows you to use `cargo run` to directly flash the pico if it's in BOOTSEL mode.

    cargo run [--release]


Hardware probe
--------------

When using [another RP2040 as a hardware probe](https://github.com/rp-rs/rp2040-project-template/blob/main/debug_probes.md)
install probe-run, and make sure `.cargo/config.toml` uses probe-run as the runner.

    cargo install probe-run

(this doesn't currently work on my setup, investigate)

License
=======

Copyright 2022 Kuno Woudt <kuno@frob.nl>

This program is free software: you can redistribute it and/or modify
it under the terms of copyleft-next 0.3.1. See
[copyleft-next-0.3.1.txt](copyleft-next-0.3.1.txt).

SPDX-License-Identifier: copyleft-next-0.3.1
