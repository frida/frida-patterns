# frida-elf-pattern

An [ImHex pattern](https://docs.werwolv.net/pattern-language) for ELF
files and images, written for the pattern compiler in Frida and checked
against real binaries in both file and memory layout.

It covers:

- 32-bit and 64-bit files of either byte order
- Program and section headers, with processor-specific types shown by machine
- The dynamic section, with tags and flags named
- Symbol tables, and relocations with per-machine types
- Symbol version definitions and requirements
- Notes from GNU, Android, FreeBSD and core dumps, and GNU properties
- The exception frame header, ARM build attributes and exception index
- MIPS ABI flags and register info

Enums name every field that has a set of known values, and the types
that matter carry a one-line summary.

## In a Frida agent

```sh
npm install frida-elf-pattern
```

```ts
import { Identification } from "frida-elf-pattern";

const header = Identification.parse(Process.mainModule.base);
console.log(header.ei_class);
```

Every struct in the pattern is exported as a class whose `parse(address)`
reads a snapshot from process memory, with enums for the fields that
have named values.

## In Luma

Install it from the Packages pane, and it shows up with the other
patterns.

## File or image layout

The pattern takes its data to be a file. When the data is an image
mapped into a process, where sections sit at their virtual addresses,
define `__MEMORY__` before importing it so offsets are translated
through the segments:

```
#define __MEMORY__
import "frida-elf-pattern/elf";
```

Luma defines it when decoding process memory.
