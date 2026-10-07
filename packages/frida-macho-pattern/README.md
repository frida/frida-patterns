# frida-macho-pattern

An [ImHex pattern](https://docs.werwolv.net/pattern-language) for Mach-O
files and images, written for the pattern compiler in Frida and checked
against real binaries in both file and memory layout.

It covers:

- Universal (fat) binaries and the images inside them
- Every load command in the macOS 27 SDK, with its own fields
- Segments and sections, with contents decoded by section type
- Symbol tables, dynamic symbol tables and indirect symbols
- Relocations for x86, x86-64, ARM and ARM64
- Code signatures: superblob, code directory, requirements and entitlements
- Exports trie, rebase and bind opcodes, and chained fixups
- Thread states, and a disassembly preview of the entry point

Enums name every field that has a set of known values, and the types
that matter carry a one-line summary.

## In a Frida agent

```sh
npm install frida-macho-pattern
```

```ts
import { MachHeader } from "frida-macho-pattern";

const header = MachHeader.parse(Process.mainModule.base);
console.log(header.ncmds);
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
import "frida-macho-pattern/macho";
```

Luma defines it when decoding process memory.
