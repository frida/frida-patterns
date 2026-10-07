# Frida patterns

[ImHex patterns](https://docs.werwolv.net/pattern-language) for the
executable formats Frida meets, published to npm for use in Frida
agents and in Luma.

| Package | Format |
| --- | --- |
| [frida-pe-pattern](packages/frida-pe-pattern) | PE and COFF |
| [frida-macho-pattern](packages/frida-macho-pattern) | Mach-O |
| [frida-elf-pattern](packages/frida-elf-pattern) | ELF |

Each package carries the `frida-pattern` keyword, which is how a host
finds patterns on npm, beside `frida-gum` like every package meant for
a Frida agent.

A pattern reads its data as a file unless `__MEMORY__` is defined, in
which case it treats the data as an image mapped into a process and
translates file offsets through the segments. Hosts decoding process
memory define it; a pattern importing one of these can define it first.

## Publishing

```sh
npm publish -w packages/<name>
```
