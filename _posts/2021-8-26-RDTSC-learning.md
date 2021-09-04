## 高级语言内嵌汇编
### 语法
- 普通写法
  - asm volatile (  
    "mov %0 %1"\n\t"  
    "add $1, %0"  
    : "=r"(dst)  
    : "r"(src)  
);
- rdtsc写法
  - **asm volatile**("rdtsc\n\t" // Returns the time in EDX:EAX.  
   "shl $32, %%rdx\n\t" // Shift the upper bits left.  
   "or %%rdx, %0"       // 'Or' in the lower   bits.  
  : "=a"(msr)  
  :  
  : "rdx");