__Control Hijacking attack__ Attacker will try and get complete control over the server. 

__Buffer overflow attacks__ Been around for a long time. Common bug which make use of many different systems. 
Common on non type safe languages like C and C++
First attack in 1988 

__Linux process memory layout__ : Every process gets 4 GB of memory. 
* Bottom is class loader. 
* Next runtime heap which grows upward 
* Shared libs. : not loaded for every process separately, but loads once in physical mem, and then maps the virtual memory to this place. 
* Stack : grows downward. 

Stack Frame for program getting executing. 
* Arguments passed to function. 
* Return address where program needs to go back. 
* Stack frame pointer of the process where this method needs to return 
* Exception Handlers
* Local variables

Attack: Example of this : Load moving Icon in Windows, Inject code into skin of media players. 
* Sring Copy never checked if the target variable had enough space. 
* The code also didn’t check that the buf as a variable is only 128 bytes. 
* The attacker doesn’t know where exactly to make the code jump to,, so what he can do is fill the return address with lots of NOP slides. And then all he needs to do is jump somewhere to the NOP slide and then NOP slide will ensure code jumps to the program.. 

NOTE: 
* Make use of methods which check for destination size before actually copying data into memory. 
* the code shouldn’t have any null char because strcpy only copies data until null char. 

__Corrupting method pointers__
Object is stored in the heap along with the methods being stored in the virtual table. 
The virtual table will consist of pointers to the actual methods. 
Because this is inheap, this one step is tricky, because the attacker doesn’t know where to point his code to. 

__Buffer overflow__
bombard program with requests, basically get it to crash, analyse dump file and then analyse the stack and heap to identify the buffer overflow vulnerability. 

__Integer over flow__

__Double free__ : can cause stability issues ,

__Format String Errors__
If method is taking input in terms of : fprintf(stderr, *user) 
Now the user can pass in format strings like “%s%s%s” in input and this will force the stack’s contents which might be sensitive data to be printed. 
User can also provide %n as argument to the method and then effectively write to the stack. 

Always provide the format string in the method itself, so that the attacker cannot pass in the format string as input.  This attacks enables the attacker to read or sometimes even write data to program memory. 

_Platform defences_
* Audit software using the automated tools. Tools like prefast,prefix.
* Making use of type safe languages is also one thing companies can do to avoid such problems.
* Doing runtime checks at some regular intervals. 

__Using hardware modules__ 
Making stack and heap as non-executable. 
OS was also updated accordingly in both Linux and Windows. Example: __DEP__ is a feature in Windows. Now when control flow tries to jump to heap or stack, it generates an alert. 
Problems: 
* every program needs to add this DEP feature to it. 
* Just In Time compilers will not be able to work with this. Because in JIT code is compiled in runtime and write to heap and code then jumps to heap.. 
* Return to libc: attacker is going to overwrite the return address to execute method in libc, and as part of argument it will be string “/bin/shell” now the attacker has tricked the program to running into shell without providing any code of his own, by executing code which is already loaded. 
Attacks on JIT: Attacker with fill up the heap with lots of executable code which might just be NOP.

__ASLR Address space Layer randomization__
As part of this shared libs are mapped to random location in process memory, so that attacker cannot jump to Exec function. 
This becomes more effective with 64 bit architecture because with 64K page size, attackers has only 256 choices to attack on

__Run time Defences__
* Stack Gaurd: Before executing return address, code will check integrity of the stack. 
* Canary: 
* * Random: Generate random string and insert it into every stack frame and saved in process memory. Verify this canary before returning from function. In case its been corrputed, crash the system. 
* * __Terminator Canary__ canary = {0, newline,linefeed, EOF}. String functions will not be able to copy beyond the terminator. Thus attacker will not be able to corrupt the return address in stack. This one only saves us from the stripy attack. 
* * * Suggested attack on Canaries: change the return address using a pointer, without having to change the canary. Though would be tricky.. 
* * * Program needs to be recompiled.  
* * * has minimalistic performance effects. 
* Static Analysis Tools 
* * PointGaurd:not much used. 
* * ProPolice: build into Gcc -stack-protector..
Rearranges the stack layout to prevent per overflow.
Local buffers are placed below canary and return address is put in over canary. Thus attacker cannot overwrite the return address without having to change the canary. 

GS : it also copies the args provided to the method to below the canary, and makes use of those canaries. Now if the attacker is changing the args, it changes the canaries too.

__Exception handlers__
If attacker is able to take control of the program before return is called, he is able to get control of the program. 
Stack has a linked list of exceptions handlers. Called SEH: Structured Exception handler frame. 
So attacker can inject a SEH, and then raise an exception so that his code gets executed.

Protecting SEH in windows env. 
* SAFESEH:a flag . Tells linker to write the addressed of valid exception handlers. Thus when the control tries to jump to a exception handler which is not in the list, program will crash.
* SEHOP: it changes how exception handlers are executed. 
Canaries are not full proof. 

__Heap Spray Attacks__
Objects that are stored on the heap. (Memeroy layout Heap, virtual table, followed by code.)
Spray the heap with NOP slide followed by shell code. And Full the whole heap with this. Now all attacker needs to do is point to a random location on heap and he will be able to get control of the program. 
Overflow string tries to copy over to the object saved before by attacker and then eventually can point to code written by attackers. 
Defense : 
* Half of the virtual address space is marked as non-writable. Virtual Table and heap will have 1 page between them, thus will end up having a non-writable memory between them. 
* Try and detect NOP slides. 


__SANDBOXing__ big defense for different attacks 
* VM’s 
* * VMM: Allow multiple guest OS on top of hardware. 
* * Android sandboxing: each process runs with its own process Id. Each applications runs as a new Delvik Virtual machine within the Phone. And static permissions are used. 
* * Linux : One os had a bug that any user can become root user by simply providing root user password file as well. 
File system sandboxing is not a good idea. 
* Sandboxing within a application. Eg : browser. 
* JVM : doesn’t allow the applets to access files on the machine, doesnt allow it to download elements from other domains. Doesn’t allow it to interact with other applets in the form. Can’t fork another process. 
Plug-ins might need to download/render data from different types. 

HTML 5 : iFrames are sandboxes. Meaning different components of. These need to be specified explicitly by the writer. Example: 
<iframe sandbox="allow-same-origin allow-scripts allow-popups allow-forms"
    src="https://platform.twitter.com/widgets/tweet_button.html"
    style="border: 0; width:130px; height:20px;"></iframe>

#### TESTING
* Program analysis for Security : Black box Scanners were able to find about half of the vulnerabilities. These tools do not completely remove all vulnerabilities. 
* Black box testing: 
* Fuzzing : randomized inputs to code and seeing what code does. Works best of a very large number of inputs. And if you testing with large numbers you might be able to find something which crashes the code. 
* Penetration testing : 
* Instrumentation of the code : 
When there is a crash or bug, one can determine which code path was excerised. Comes with performance cost. 
* * Purify : Instrument the memory of program to detect run-time memory errors. 
* * Perl Tainting : Information flow tracking. Tracks at runtime the data that has been provided to the program. And check if any formatting etc was done on the data before making a system call etc. the tool is not able to determine if the tests being performed by the code are sufficient in checking the inputs.
* Data Race Concurrency: 
Two threads might access the memory at same time. Can lead to data corruption, crashes etc. 
Here the instrumentation codes are trying to determine if there is a potential for concurrent access. 
From the structure of the code there is a order. Determine a Happens-Before order for a program. Conflict occurs if there are 2 access to a variable at same time and at least 1 of them is a write operation. Race condition : if there are 2 access from 2 different threads and they are in conflict and they are not ordered by a happens before relationship. 
* __Lockset algorithm__ checks for locks while accessing variables in multithreaded env. 
* __Static code analysis__ They look at the code and are able to determine any issues with the code. 
Testing can find presence of some bugs, it can never confirm absence of bugs. Because usually there are thousands of possible combinations of the code. And Static analysis tools would help with determining correctness.

Goals of Static code analysis :
* _Soundness_: If the program contains an error, the analysis with report a warning. 
“Sound for reporting correctness”
* _Completeness_ : if the analysis reports an error, the program will contain an error
“Complete for reporting correctness”
_Undecidable_: theoretically not possible Sound and complete reports all error and reports no false alarms 

Program analyzers : 
Code + spec 
Would need the tools to scale up. 
tools might generate false alarms, and too many of these can be difficult to keep track of..   
 
_Abstracts state analysis_
We want to keep track of different state of program in defined enough ways to be able to analyse the flow of the program without having to go through the whole flow in complete detail. 
If the descriptions being used in analysis are more detailed analysis will take much longer, otherwise they might loose imp info needed to be able to determine the flow 

* Chroot : jail a process into the subdirectories 
* Tainting checkers: taint data from source and then determine if there are vulnerabilities resulting from using tainted data in source code.. 
* Reverse Topological sort : to analyse each function before its caller method is analyzed,. Analyse things from bottom up. Called modular code analysis. 
* Path traversal : analyse each path of code separately and then analyse code at join points together. 
* User pointer inference: first compute all the tainted pointers. 



Languages to write code in based on security vulnerabilities: RUST (lets try and learn it)


