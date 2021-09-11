# Getting Started

Before running mumu, you need to meet some prerequisites. Once you're sure you've provided them, you can start mumu.

| Hardware  | Minimum Requirement                                       |
|-----------|-----------------------------------------------------------|
| Processor | x86 64-bit processor and VT-x Technology must be enabled  |
| Memory    | 4 GB                                                      |

You need a Linux machine to run mumu. Also, check your hardware support for VT-x technology.

```shell
cat /proc/cpuinfo | grep vmx
```

## Important Header Files

```go
#include "../lib/hypervisor/kvm/kvm.h"
#include "../lib/hypervisor/kvm/vm.h"
#include "../lib/hypervisor/kvm/vcpu.h"
```

mumu needs three core header files to create virtual machines.


## Creating VM's
It's very easy to create VM's inside mumu. First, user should connect to the KVM with KVM class.

```cpp
KVM *_kvm = new KVM();
_kvm->init();
```

After, user can create virtual machines.
```cpp
VM *vm = new VM(_kvm);
vm->create_vcpu(0, 0, 0);
vm->mem_init();
```


## Loading Binaries
It's very easy to load binary files to virtual machines in mumu. There is a function called `load_text_binary(char* hex)` which takes a binary as char array and load into virtual machine or you can load from file:

```cpp
load_binary_data("hello_world.bin");
```


## Loading Elf Files
It is possible to inject elf files directly into virtual machine. it is also important that the C files are compiled correctly. First of all, you should remember that the program must be compiled with static linking.

```cpp
load_elf("/home/mumu_visor/Hello World/IO Devices/prog",vm);
auto s_address = get_entry_address("/home/mumu_visor/Hello World/IO Devices/prog");
```

## Setup Mode
There are 3 different modes that mumu can support:

* Real Mode
* 32 bit Protected Mode
* 64 bit Long Mode

```cpp
run_long_mode(vm,vm->vcpus[0],s_address);
```

## Run Virtual Machine

It's possible to run and stop virtual machine with simple functions. 

```
vm->run_vm();
vm->stop_vm();
```

## Simple Hello World Program
```cpp
#include "../lib/hypervisor/kvm/kvm.h"
#include "../lib/hypervisor/kvm/vm.h"
#include "../lib/hypervisor/kvm/vcpu.h"
#include "../lib/hypervisor/kvm/modes.h"
#include "../lib/hypervisor/loader/mumu_elf.hpp"

int main()
{
    KVM *_kvm = new KVM();
    _kvm->init();

    VM *vm = new VM(_kvm);
    vm->create_vcpu(0, 0, 0);
    vm->mem_init();

    auto s_address = load_elf("./helloworld",vm);

    run_long_mode(vm,vm->vcpus[0],s_address);
    vm->run_vm();
    vm->stop_vm();
}
```
Output:

```t
------- mumu started ----------
KVM version 12
VM created 
VCPU init success
Testing 64-bit mode
core: 0 140737341830912
-------------------------------
CPU init with id: 324977 core: 0
id: 324977 core: 0 id: 0 data: H
id: 324977 core: 0 id: 0 data: e
id: 324977 core: 0 id: 0 data: l
id: 324977 core: 0 id: 0 data: l
id: 324977 core: 0 id: 0 data: o
id: 324977 core: 0 id: 0 data:  
id: 324977 core: 0 id: 0 data: W
id: 324977 core: 0 id: 0 data: o
id: 324977 core: 0 id: 0 data: r
id: 324977 core: 0 id: 0 data: l
id: 324977 core: 0 id: 0 data: d
halted
```