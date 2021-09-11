# Examples

## Loading x86 Assembly
In mumu you can load different type of x86 assemblies. Currently mumu starts in long mode. So you should start the operating system with 32bit or 64 bit mode.

### 16 bit Real Mode
To enable 16 bit real mode you should remove `page_table` configurations. Sample program:

```s
.globl _start
    .code16
_start:
    xorw %ax, %ax
    
loop:
    out %ax, $0xf1
    inc %ax
    jmp loop
```

assemble this program and extract text section. 

### 64 bit Long Mode
```s
_start:
    xorw %dax, %dax
    
loop:
    out %dax, $0xf1
    inc %dax
    jmp loop

```



## Loading Simple C Program

It's possible to run bare metal C programs inside mumu. Also, there is a `common.h` library that programmers can be use. To run bare metal applications you should avoid of dynamic linking and external libraries (it's ok when you compile with static linking). 

Program:

```c
#include "common.h"

void main(){
    char a[5] = {'H','e','l','l','o'};
    for(int k = 0; k < 5; k++){
        write(a[k]);
    }
}
```

Directory and makefile
```
cd examples/hello-world
make
```

Output:
```
Hello World !
```

## Using XML File 

Create a XML parser.

```c++
xml_parser* parser = new xml_parser();
auto parsed_vms =  parser->parse("/home/musa/test.xml");
```

Set up VMs with parsed data:

```c++
auto vm_list = new list<VM*>();
for(auto l: *parsed_vms){
    VM *vm = new VM(_kvm);
    for(auto k = 0; k < l->cores.size(); k++){
        vm->create_vcpu(k,l->cores[k]->core_id,l->cores[k]->physical_core_map);
    }        
    vm->mem_init();
    vm->load_binary(l->binary_path);
    l->vm = vm;
    vm_list->push_back(vm);
}
```

## Static Scheduling
In order to use static scheduler you should parse information from XMl file. 

```c++
#include <iostream>

auto sched_info = parser->generate_core_scheduling_order(parser->windows);

vector<Scheduler*> scheduler_list;
for(auto core = 0; core<4; core++){
    Scheduler* sch = new Scheduler(&sched_info[core],core);
    scheduler_list.push_back(sch);
}
```


## Multicore Application

## Using Serial Console




## Shared Memory

## Inter VM Communication

### Sampling Port