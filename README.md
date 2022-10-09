# ProcessorTestsBinaries

The test sets for variants of the 6502, available at [TomHarte/ProcessorTests](https://github.com/TomHarte/ProcessorTests),
are provided in a convenient human-readable JSON format but are quite large wrt. storage 
requirements. To reduce file size and increase processing speed a script was created 
that converts the files from JSON to a binary layout format that simplifies processing
in languages such as C. The script to perform the conversion and converted binaries of 
some of the tests are included with this repository.

## Binary format

The following format is repeated for all 10000 tests.

| Bytes |           | Description                       |
|-------|:----------|-----------------------------------|
| 10    | Name      | Null terminated string            |
|       |           | Initial State Block               |
| 2     | PC        | Program Counter                   |
| 1     | S         | Stack Pointer                     |
| 1     | A         | Accumulator                       |
| 1     | X         | X index register                  |
| 1     | Y         | Y index register                  |
| 1     | P         | CPU Flags                         |
| 1     | n         | RAM entry count                   |
|       |           | RAM Entries                       |
| 2     | Addr      | RAM entry address                 |
| 1     | Value     | RAM entry value                   |
|       |           | ...                               |
|       |           | Final State Block                 |
| 2     | PC        | Program Counter                   |
| 1     | S         | Stack Pointer                     |
| 1     | A         | Accumulator                       |
| 1     | X         | X index register                  |
| 1     | Y         | Y index register                  |
| 1     | P         | CPU Flags                         |
| 1     | n         | RAM entry count                   |
|       |           | RAM Entries                       |
| 2     | Addr      | RAM entry address                 |
| 1     | Value     | RAM entry value                   |
|       |           | ...                               |
|       |           | Cycles Block                      |
| 1     | n         | Cycle Entry Count                 |
| 2     | Addr      | Address accessed                  |
| 1     | Value     | Value read or written             |
| 1     | Operation | Char 'r' or 'w' for Read or Write |
|       |           | ...                               |


## pcjson2bin.py

This script converts a JSON test file to a binary version in-place. It will check for
files in the current directory that looks like `xx.json` where `xx` is a hexadecimal 
value between `00` and `FF`. It automatically creates an empty bin file if the JSON
is empty (for example CB.json and DB.json in WDC65C02).

