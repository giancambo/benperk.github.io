# Debugging a hung application with WinDbg

There are many reasons that threads can be blocked causing an application to hang.  This is an example of one of those situations.  I loaded a memory dump into WinDbg and loaded the PSSCOR2 extension.  When I executed !syncblk,  I received the following result shown in the following.

```
0:000> !syncblk

Index  SyncBlock  MonitorHeld  Recursion  Owning    Thread  Info  SyncBlock  Owner
546    16e2f11c   9            1          0a1df058  23a78   18    02b133e4   System.Object
Waiting   threads: 0 24 77 80
```

To me, the result means that Thread 18 is blocking threads 0, 24, 77 and 80.  I execute !threads and get the result shown below.  For this example, I restricted the output just to those threads.

```
0:018> !threads

ID    PreEmptive   GC       Domain        Lock Count      APT
0     Enabled               0066ec18      0               MTA    (Threadpool Worker)
18    Enabled               0066ec18      1               MTA    (Threadpool Worker)
24    Enabled               0066ec18      0               MTA    (Threadpool Worker)
77    Enabled               0066ec18      0               MTA    (Threadpool Worker)
80    Enabled               0066ec18      0               MTA    (Threadpool Worker)
```

Next I want to see what is happening on one of the threads that is being blocked.  I might be able to find what the thread is waiting on.  So I enter a ~80s command which gets that thread in focus and then look at the call stack as illustrated below.  What I notice is a Generic Dictionary that is being searched using the ContainsKey() method.

```
0:080> k

ChildEBP   RetAddr
2ca8ebfc   76c315e9    ntdll!NtWaitForMultipleObjects+0x15
2ca8ec98   76061a2c    KERNELBASE!WaitForMultipleObjectsEx+0x100
2ca8ece0   59d445a2    kernel32!WaitForMultipleObjectsExImplementation+0xe0
2ca8ed48   59d441cf    mscorwks!WaitForMultipleObjectsEx_SO_TOLERANT+0x6f
2ca8ed68   59d442d8    mscorwks!Thread::DoAppropriateAptStateWait+0x3c
2ca8edec   59d4436d    mscorwks!Thread::DoAppropriateWaitWorker+0x13c
2ca8ee3c   59d444f1    mscorwks!Thread::DoAppropriateWait+0x40
2ca8ee98   59be5402    mscorwks!CLREvent::WaitEx+0xf7
2ca8eeac   59d198ea    mscorwks!CLREvent::Wait+0x17
2ca8ef38   59d436f0    mscorwks!AwareLock::EnterEpilog+0x8c
2ca8ef54   59d19e78    mscorwks!AwareLock::Enter+0x61
2ca8efbc   59d19c57    mscorwks!AwareLock::Contention+0x199 
2ca8f05c   085c0707    mscorwks!JITutil_MonContention+0xa3 
2ca8f098   025324e6    ***_Collections!***.Generic.Dictionary`2.ContainsKey(System.__Canon)+0x4f
…
```

Next I want to look at the JIT generated code for mscorwks!JITutil_MonContention+0xa3.  Using the !u command, I can view this, as shown below.  I reduced the output, but you can see the ContainsKey() method is contained within a Monitor.Enter() and a Monitor.Exit() method.

```
0:080> !u 085c0707

….
085c06ff    8b4dcc          mov      ecx,dword ptr [ebp-34h]
085c0702   e89e246251       call       mscorwks!JIT_MonEnterWorker (59be2ba5)
>>>   085c0707 8b45d4       mov     eax,dword ptr [ebp-2Ch]
085c070a  8b4804            mov      ecx,dword ptr [eax+4]
…
085c0712  e8e9402670        call       Collections.Generic.Dictionary`2.ContainsKey(…)+..0f(…)
…
085c0737  8b4dcc            mov      ecx,dword ptr [ebp-34h]
085c073a  e8e0266251        call    mscorwks!JIT_MonExitWorker (59be2e1f)
085c073f   58               pop     eax
085c0740   ffe0             jmp     eax
```

Now let’s go check what is happening on the thread blocking 80.  Execute ~18s and then k to get the stack shown next.

```
0:018> k

ChildEBP  RetAddr
0b1ee410  76c315e9   ntdll!NtWaitForMultipleObjects+0x15
0b1ee4ac  76061a2c   KERNELBASE!WaitForMultipleObjectsEx+0x100
0b1ee4f4  59d445a2   kernel32!WaitForMultipleObjectsExImplementation+0xe0
0b1ee55c  59d441cf   mscorwks!WaitForMultipleObjectsEx_SO_TOLERANT+0x6f
0b1ee57c  59d442d8   mscorwks!Thread::DoAppropriateAptStateWait+0x3c
0b1ee600  59d4436d   mscorwks!Thread::DoAppropriateWaitWorker+0x13c
0b1ee650  59ce4af8   mscorwks!Thread::DoAppropriateWait+0x40
0b1ee754  788069df   mscorwks!WaitHandleNative::CorWaitOneNative+0x156
0b1ee770  78806995   mscorlib_ni!System.Threading.WaitHandle.WaitOne(Int64,  Boolean)+0x2f
0b1ee788  7b4b2231   mscorlib_ni!System.Threading.WaitHandle.WaitOne(Int32,  Boolean)+0x25
0b1ee7a4  7af94c20   ***_ni!***.Control.WaitForWaitHandle(System.Threading.WaitHandle)+0x89
…
0b1ee960  04e32a2f   ***_Collections!***.Generic.Dictionary`2.OnItemChanged.(…)+0xad
0b1ee99c  02531d10   ***_Collections!***.Generic.Dictionary`2.set_Item(…)+0x6f
```

Hmmm, interesting, it appears that this thread is writing something to the Generic.Dictionary.

Further analysis of the code resulted in identifying the fact that the Generic.Dictionary was static and all the threads in this scope where accessing the same dictionary.

***Note***

Check out this [article][LINK1] where it discusses the thread safety of a Dictionary.  Basically, a dictionary can support concurrent readers are long as the collection is not modified.  Which we see in this case, that the dictionary is being modified which is blocking the other threads that are trying to read from it.

***Solution***

The [solution][LINK2] is to synchronize your threads, making sure that there will not be a case where the dictionary is written to and read from at the same time.

[LINK1]: http://msdn.microsoft.com/en-us/library/xfhwa508(VS.80).aspx
[LINK2]: http://msdn.microsoft.com/en-us/library/ms173179(v=VS.80).aspx
