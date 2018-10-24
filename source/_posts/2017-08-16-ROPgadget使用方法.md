# ROPgadget 使用方法


## usage: 
ROPgadget [-h] [-v] [-c] [--binary <binary>] [--opcode <opcodes>]
                 [--string <string>] [--memstr <string>] [--depth <nbyte>]
                 [--only <key>] [--filter <key>] [--range <start-end>]
                 [--badbytes <byte>] [--rawArch <arch>] [--rawMode <mode>]
                 [--re <re>] [--offset <hexaddr>] [--ropchain] [--thumb]
                 [--console] [--norop] [--nojop] [--callPreceded] [--nosys]
                 [--multibr] [--all] [--dump]

## description:
  ROPgadget lets you search your gadgets on a binary. It supports several 
  file formats and architectures and uses the Capstone disassembler for
  the search engine.

## formats supported: 
  - ELF
  - PE
  - Mach-O
  - Raw

## architectures supported:
  - x86
  - x86-64
  - ARM
  - ARM64
  - MIPS
  - PowerPC
  - Sparc

## optional arguments:
  -h, --help           show this help message and exit
  -v, --version        Display the ROPgadget's version
  -c, --checkUpdate    Checks if a new version is available
  --binary <binary>    Specify a binary filename to analyze
  --opcode <opcodes>   Search opcode in executable segment
  --string <string>    Search string in readable segment
  --memstr <string>    Search each byte in all readable segment
  --depth <nbyte>      Depth for search engine (default 10)
  --only <key>         Only show specific instructions
  --filter <key>       Suppress specific instructions
  --range <start-end>  Search between two addresses (0x...-0x...)
  --badbytes <byte>    Rejects specific bytes in the gadget's address
  --rawArch <arch>     Specify an arch for a raw file
  --rawMode <mode>     Specify a mode for a raw file
  --re <re>            Regular expression
  --offset <hexaddr>   Specify an offset for gadget addresses
  --ropchain           Enable the ROP chain generation
  --thumb              Use the thumb mode for the search engine (ARM only)
  --console            Use an interactive console for search engine
  --norop              Disable ROP search engine
  --nojop              Disable JOP search engine
  --callPreceded       Only show gadgets which are call-preceded
  --nosys              Disable SYS search engine
  --multibr            Enable multiple branch gadgets
  --all                Disables the removal of duplicate gadgets
  --dump               Outputs the gadget bytes

## examples:

测试用例：


    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 
    #获取全部binary下的gadget组件
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --ropchain
    #形成rops链
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --depth 3
    #遍历深度为3，即只是遍历3层深度
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --string "main"
    #在出现main字符串的周围检索
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --string "m..n"
    #在m..n这种字符串周围检索gadgets
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --opcode c9c3
    #操作码是c9c3 
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --only "mov|ret"
    #包含mov|ret 这种模式的
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --only "mov|pop|xor|ret"
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --filter "xchg|add|sub"
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --norop --nosys
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --range 0x08041000-0x08042000
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --string main --range 0x080c9aaa-0x080c9aba
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --memstr "/bin/sh"
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --console
    ROPgadget.py --binary ./test-suite-binaries/elf-Linux-x86 --badbytes "00|01-1f|7f|42"
    ROPgadget.py --binary ./test-suite-binaries/Linux_lib64.so --offset 0xdeadbeef00000000
    ROPgadget.py --binary ./test-suite-binaries/elf-ARMv7-ls --depth 5
    ROPgadget.py --binary ./test-suite-binaries/elf-ARM64-bash --depth 5
    ROPgadget.py --binary ./test-suite-binaries/raw-x86.raw --rawArch=x86 --rawMode=32

