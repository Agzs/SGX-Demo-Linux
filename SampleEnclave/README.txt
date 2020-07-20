Enclave 开发基础(一) 基本开发步骤：https://zhuanlan.zhihu.com/p/39914622
Enclave 开发基础(二) 语法定义：https://zhuanlan.zhihu.com/p/39914846
Enclave 开发基础(三) 文件和工程配置：https://zhuanlan.zhihu.com/p/39917377
Enclave 开发基础 SampleEnclave示例代码分析：https://zhuanlan.zhihu.com/p/39973868

------------------------
Purpose of SampleEnclave
------------------------
The project demonstrates several fundamental usages of Intel(R) Software Guard 
Extensions (Intel(R) SGX) SDK:
- Initializing and destroying an enclave 初始化和销毁一个飞地
- Creating ECALLs or OCALLs 创建ECALL和OCALL
- Calling trusted libraries inside the enclave 飞地内调用可信库

------------------------------------
How to Build/Execute the Sample Code
------------------------------------
1. Install Intel(R) SGX SDK for Linux* OS
2. Make sure your environment is set:
    $ source ${sgx-sdk-install-path}/environment
3. Build the project with the prepared Makefile:
    a. Hardware Mode, Debug build:
        $ make
    b. Hardware Mode, Pre-release build:
        $ make SGX_PRERELEASE=1 SGX_DEBUG=0
    c. Hardware Mode, Release build:
        $ make SGX_DEBUG=0
    d. Simulation Mode, Debug build:
        $ make SGX_MODE=SIM
    e. Simulation Mode, Pre-release build:
        $ make SGX_MODE=SIM SGX_PRERELEASE=1 SGX_DEBUG=0
    f. Simulation Mode, Release build:
        $ make SGX_MODE=SIM SGX_DEBUG=0
4. Execute the binary directly:
    $ ./app
5. Remember to "make clean" before switching build mode

------------------------------------------
Explanation about Configuration Parameters
------------------------------------------
TCSMaxNum, TCSNum, TCSMinPool

    These three parameters will determine whether a thread will be created
    dynamically  when there is no available thread to do the work.


StackMaxSize, StackMinSize

    For a dynamically created thread, StackMinSize is the amount of stack available
    once the thread is created and StackMaxSize is the total amount of stack that
    thread can use. The gap between StackMinSize and StackMaxSize is the stack
    dynamically expanded as necessary at runtime.

    For a static thread, only StackMaxSize is relevant which specifies the total
    amount of stack available to the thread.


HeapMaxSize, HeapInitSize, HeapMinSize

    HeapMinSize is the amount of heap available once the enclave is initialized.

    HeapMaxSize is the total amount of heap an enclave can use. The gap between
    HeapMinSize and HeapMaxSize is the heap dynamically expanded as necessary
    at runtime.

    HeapInitSize is here for compatibility.

-------------------------------------------------    
Sample configuration files for the Sample Enclave
-------------------------------------------------
config.01.xml: There is no dynamic thread, no dynamic heap expansion.
config.02.xml: There is no dynamic thread. But dynamic heap expansion can happen.
config.03.xml: There are dynamic threads. For a dynamic thread, there's no stack expansion.
config.04.xml: There are dynamic threads. For a dynamic thread, stack will expanded as necessary.

-------------------------------------------------
Launch token initialization
-------------------------------------------------
If using libsgx-enclave-common or sgxpsw under version 2.4, an initialized variable launch_token needs to be passed as the 3rd parameter of API sgx_create_enclave. For example,

sgx_launch_token_t launch_token = {0};
sgx_create_enclave(ENCLAVE_FILENAME, SGX_DEBUG_FLAG, launch_token, NULL, &global_eid, NULL);
