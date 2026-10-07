# frida-pe-pattern

An [ImHex pattern](https://docs.werwolv.net/pattern-language) for PE
files and images, written for the pattern compiler in Frida and checked
against real binaries in both file and memory layout.

It covers:

- DOS header, Rich header and COFF file header
- PE32 and PE32+ optional headers and section headers
- Exports, imports, delay imports and bound imports
- Resources, with version info, strings, icons and bitmaps decoded
- Exception tables for x64, ARM64 and ARM
- Base relocations with per-machine types, and debug directories
- TLS, load configuration with its guard tables, and the CLR header
- Certificates, and COFF object files with symbols and relocations

Enums name every field that has a set of known values, and the types
that matter carry a one-line summary.

## In a Frida agent

```sh
npm install frida-pe-pattern
```

```ts
import { DosHeader } from "frida-pe-pattern";

const header = DosHeader.parse(Process.mainModule.base);
console.log(header.e_lfanew);
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
import "frida-pe-pattern/pe";
```

Luma defines it when decoding process memory.
