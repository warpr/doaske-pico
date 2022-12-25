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


Flash directly from a PI
------------------------

If we're building on a Raspberry Pi, we can flash directly via the GPIO pins.  This is
described in more detail in the "Getting started with Raspberry Pi Pico" official
book? documentation? from Raspberry Pi, Chapter 5: Flash Programming with SWD.

Wiring:

    Pin 1  (3V3)               ->      3V3       (if powering the pico from the pi)
    Pin 6  (GND)               ->      GND
    Pin 8  (GPIO14, UART0 TX)  ->      Pin 2 (GPIO1, UART0 RX)
    Pin 9  (GPIO15, UART0 RX)  ->      Pin 1 (GPIO0, UART0 TX)
    Pin 18 (GPIO24)            ->      SWDIO
    Pin 20 (GND)               ->      SWD GND
    Pin 22 (GPIO25)            ->      SWDCLK


If the Pico has been wired to the Pi, the following command can be used to flash the elf
binary directly to the Pico.

   bin/gpio-flash

NOTE: this script calls openocd, which was built directly from git, following the instructions in the previously mentioned Chapter 5.


Printf debugging
----------------

If UART has been connected according to the above diagrom, minicom can be used to connect to it
like this:

    minicom --baudrate 115200 --device /dev/ttyS0

(On some pis the device may be `/dev/ttyAMA0`)

This has been saved to a script at `bin/uart`.

NOTE: minicom does NOT treat a newline as a newline + carriage return by default, but
this can be enabled by saving the following line in `~/.minirc.dfl`:

    pu addcarreturn    Yes


License
=======

Copyright 2022 Kuno Woudt <kuno@frob.nl>

This program is free software: you can redistribute it and/or modify
it under the terms of copyleft-next 0.3.1. See
[copyleft-next-0.3.1.txt](copyleft-next-0.3.1.txt).

SPDX-License-Identifier: copyleft-next-0.3.1
