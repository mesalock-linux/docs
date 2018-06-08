# About MesaLock Linux

## What is MesaLock Linux?

MesaLock Linux is a general purpose Linux distribution which aims to provide a
**safe** and **secure** user space environment. To eliminate high-severe
vulnerabilities caused by memory corruption, the whole user space applications
are rewritten in **memory-safe** programming languages like Rust and Go.

This extremely reduces attack surfaces of an operating system exposed in the
wild, leaving the remaining attack surfaces *auditable* and *restricted*.
Therefore, MesaLock Linux can substantially improve the security of the Linux
ecosystem. Additionally, thanks to the Linux kernel, MesaLock Linux supports a
broad hardware environment, making it deployable in many places.

Two main usage scenarios of MesaLock Linux are containers and security-sensitive
embedded devices. With the growth of the ecosystem, MesaLock Linux would also be
adopted in the server/cloud environment.

## Memory safety?

Fatal bugs introduced by non-memory-safe languages (C/C++/etc.) are one of the
oldest yet persistent problems in computer security. By using memory-safe
programming languages like Rust and Go, developers can obtained guarantees of
type soundness, memory safety, and thread safety. We believe that using
memory-safe programming languages will eliminate memory issues and provide a
safe and secure environment. Therefore, we decide to focus on providing a
memory-safe Linux distribution.


## Sister projects

At last, if you care about the memory safety, you may also interested in our
sister projects:
  - [MesaLink](https://github.com/mesalock-linux/mesalink): a memory-safe and
    OpenSSL-compatible TLS library
  - [Rust SGX SDK](https://github.com/baidu/rust-sgx-sdk): an SDK helps
    developers write Intel SGX applications in Rust programming language
