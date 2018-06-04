# Introduction

MesaLock Linux is a general purpose Linux distribution which aims to provide a
*safe* and *secure* user space environment. To eliminate high-severe
vulnerabilities caused by memory corruption, the whole user space applications
are rewritten in *memory-safe* programming languages like Rust and Go. This
extremely reduces attack surfaces of an operating system exposed in the wild,
leaving the remaining attack surfaces *auditable* and *restricted*. Therefore,
MesaLock Linux can substantially improve the security of the Linux ecosystem.
Additionally, thanks to the Linux kernel, MesaLock Linux supports a broad
hardware environment, making it deployable in many places. Two main usage
scenarios of MesaLock Linux are containers and security-sensitive embedded
devices. With the growth of the ecosystem, MesaLock Linux would also be adopted
in the server/cloud environment.
