# Reference

!!! Warning
    This section is under development. 


## KVM Functions

### `init()`

```
int KVM::init();
```
**Returns**: file descriptor of KVM instance

### `get_fd()`

```
uint32_t KVM::get_fd() const;
```
**Returns**: file descriptor of KVM instance

### `get_version()`

```
uint32_t KVM::get_version() const;
```
**Returns**: version of KVM instance



## VM Functions

### `VM()`

```
VM(KVM *_ref);
```
**Returns**: VM object

**Description**: Creates VM with `KVM_CREATE_VM` ioctl

### `VCPU* create_vcpu()`

```cpp
VCPU* create_vcpu(int, uint8_t, uint8_t)
```
**Returns**: VCPU object

**Args**: int _id, uint8_t _virtual_core, uint8_t _physical_core

**Description**: Creates VM with `KVM_CREATE_VM` ioctl

### `load_binary()`

```
void load_binary(const string _file);
```
**Returns**: none

**Description**: Load binary from a file

### `load_text_binary()`

```
void load_text_binary(uint8_t[],int);
```
**Returns**: none

**Description**: Load binary from char array

### `mem_init()`

```
int mem_init();
```
**Returns**: 1 on success -1 on error

**Description**: Initialize memory region for VM.


## VCPU Functions

### `VCPU()`

```
VCPU(){};
```
**Returns**: VCPU object

**Description**: Default constructor.

### `VCPU(...)`

```
VCPU(int, int, struct kvm_run*,int)
```
**Returns**: VCPU object

**Description**: Extended constructor.

### `run()`

```
void run(int,int);
```
**Returns**: none

**Args**: core, sec

**Description**: Run VCPU thread.

### `run_real_mode()`

```
int run_real_mode();
```
**Returns**: -1 on error 1 on success

**Description**: Configure VCPU for running on real mode

### `run_protected_mode()`

```
int run_protected_mode();
```
**Returns**: -1 on error 1 on success

**Description**: Configure VCPU for running on protected mode


### `set_cores()`

```
void set_cores(int, int);
```
**Returns**: none

**Args**: physical core, virtual core

**Description**: Configure VCPU cores

### `setup_segment_registers()`

```
void setup_segment_registers();
```
**Returns**: none

**Description**: Configure segment registers for 32 bit and 64 mode

### `page_tables()`

```
void page_tables(void* mem, struct kvm_sregs *sregs)
```
**Returns**: none

**Args**: memory region, register struct

**Description**: Setup page tables for 64bit long mode


