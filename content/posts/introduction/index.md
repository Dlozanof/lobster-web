+++
title = 'git commit -m "Initial commit"'
date = 2024-03-14T07:07:07+01:00
+++

---

# Renode
I plan to use real hardware, but wanted to have Renode running to easily check changes without handling boards. It was rather easy to setup. The only notable point is that I created a script to automatically load the code.

```
:name: Mi-V
:description: Debugging script.

$name?="Mi-V"

mach create $name
machine LoadPlatformDescription @platforms/boards/stm32f4_discovery.repl

$bin?=@/whatever/lobster/target/thumbv7em-none-eabihf/debug/lobster

macro reset
"""
    sysbus LoadELF $bin
"""
runMacro $reset
machine StartGdbServer 3333
```


Which I can run with
```bash
renode --hide-monitor renode_start.resc
```

And then connect to it via GDB
```bash
arm-none-eabi-gdb target/thumbv7em-none-eabihf/debug/lobster
```

# Minimal setup
Let us try to have a hello world example going on.... My plan is to leverage [stm32-rs](https://github.com/stm32-rs) project. Specifically it seems there is a readily available [BSP](https://github.com/stm32-rs/stm32f407g-disc) I could use.

RTIC seems to use this approach, and I might be able to do the same. Let's see.

For now let's try to follow the memfault guide for bare metal rust booting (1). Or not. Right out of the bat I see there are several undocumented parts so I will switch to another source. Let's see if any of the official works of the Embedded [workgroup](https://github.com/rust-embedded/wg) is ok.
- https://docs.rust-embedded.org/book/
- https://docs.rust-embedded.org/
- https://docs.rust-embedded.org/embedonomicon/

For now I see I need several things. First one of them is a recent version of `rustc`. I find the [rustc book](https://doc.rust-lang.org/rustc/what-is-rustc.html) interesting to know what the fuck should I do. Like updating. But using `rustup`. In this case I did `check` and `update`.

I also seem to need [`cargo-binutils`](https://github.com/rust-embedded/cargo-binutils) and [`cargo-embed`](https://probe.rs/docs/tools/cargo-embed/) . They look nice so I do install them.


References:
* (1) https://interrupt.memfault.com/blog/zero-to-main-rust-1
* https://github.com/rust-embedded/awesome-embedded-rust#hal-implementation-crates
