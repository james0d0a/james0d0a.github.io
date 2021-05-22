---
title: "Rust Hello World on Teensy 4.0"
date: 2020-05-20T19:35:00-08:00
categories:
  - blog
tags:
  - functional_programming
  - rust
  - teensy
---

![teensy blink](/assets/images/teensy-blink.gif)

This is just a quick post to jot down some notes on getting the embedded rust tool chain working on a Teensy 4.0.  I was pleasantly surprised at how easy it was and the biggest hiccup was a bad usb cable.

## Requirements

* Rust installed
* The following rust arm target
`$rustup target add thumbv7em-none-eabihf`
* install cargo bin-utils
`$ cargo install cargo-binutils`
`$rustup component add llvm-tools-preview`

Note: I missed installing the llvm-tools-preview in the instructions initially but cargo objcopy wont work w/o it.  you will get the following error: `Failed to execute tool: objcopy No such file or directory (os error 2)`



## Cargo template for hello wordl
* `cargo install cargo-generate`
* `cargo generate --git https::/github.com/mciantyre/teensy4-rs-template --name hello-world`
* `cd hello-world`
* `cargo objcopy --release -- -O ihex hello-world.hex`


## teensy loader on linux
* download or compile teensy loader or teensy loader cli from [PJRC website](https://www.pjrc.com/teensy/loader_linux.html)
* [install udev rules](https://www.pjrc.com/teensy/00-teensy.rules) to /etc/udev/rules.d/ so you can avoid using sudo/root every time you load code onto the teensy.
* connect teensy via usb cable run the the teensy loader and press button on teensy to enter program mode and load the hex file.
![teensy loader](/assets/images/teensy-loader.png)

here is the hello world code from the cargo template our first rust embedded program!


```rust
//! The starter code slowly blinks the LED, and sets up
//! USB logging.

#![no_std]
#![no_main]

use teensy4_bsp as bsp;
use teensy4_panic as _;

mod logging;

const LED_PERIOD_MS: u32 = 1_000;

#[cortex_m_rt::entry]
fn main() -> ! {
    let p = bsp::Peripherals::take().unwrap();
    let mut systick = bsp::SysTick::new(cortex_m::Peripherals::take().unwrap().SYST);
    let pins = bsp::t40::into_pins(p.iomuxc);
    let mut led = bsp::configure_led(pins.p13);

    // See the `logging` module docs for more info.
    assert!(logging::init().is_ok());

    loop {
        led.toggle();
        systick.delay(LED_PERIOD_MS);
        log::info!("Hello world");
    }
```

## Additional resources and reading
* [Overview of Embedded Rust Ecosystem](https://www.youtube.com/watch?v=vLYit_HHPaY)
* [Why the teensy is awesome](https://www.youtube.com/watch?v=75IvTqRwNsE)
* [Awesome Embedded Rust](https://github.com/rust-embedded/awesome-embedded-rust)