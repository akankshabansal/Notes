### Static 
We can have a simple static block inside a class, which is called the first thing when the class gets loaded.. 

    public class Test {
        static {
            System.out.println("inside static block\n");
        }
        public static void main(String[] args) {
        }
    }

### Annotations 
Annotations, a form of metadata, provide data about a program that is not part of the program itself. Annotations have no direct effect on the operation of the code they annotate.

Annotations have a number of uses, among them:

**Information for the compiler** — Annotations can be used by the compiler to detect errors or suppress warnings.

**Compile**-time and deployment-time processing — Software tools can process annotation information to generate code, XML files, and so forth.

**Runtime processing** — Some annotations are available to be examined at runtime.

Annotations can be used to replace comments and create meaningful documentation out of them. 
They can be used to do Type checking for you. 

Example : 
You can write or dowload a peice of code which checks if a perticular variable is never null. 
You need to write the annotation before declaring the variable and define the behaviour to the compiler.. 
There are third parties who have written such peices of code for you.. 


**_@Deprecated_** indicates that the marked element is deprecated and should no longer be used. The compiler generates a warning whenever a program uses a method, class, or field with the @Deprecated annotation. 

**_@Override_** @Override annotation informs the compiler that the element is meant to override an element declared in a superclass. It helps to prevent errors by creating an compilation error if method fails to correctly override a method in one of its superclasses.

**_@SuppressWarnings_** @SuppressWarnings annotation tells the compiler to suppress specific warnings that it would otherwise generate.

**_@FunctionalInterface_** @FunctionalInterface annotation, introduced in Java SE 8, indicates that the type declaration is intended to be a functional interface, as defined by the Java Language Specification.


### Cool annotations or meta-annotations. 

**_@Retention_** @Retention annotation specifies how the marked annotation is stored:

1. RetentionPolicy.SOURCE – The marked annotation is retained only in the source level and is ignored by the compiler.
2. RetentionPolicy.CLASS – The marked annotation is retained by the compiler at compile time, but is ignored by the Java Virtual Machine (JVM).
3. RetentionPolicy.RUNTIME – The marked annotation is retained by the JVM so it can be used by the runtime environment.

**_@Documented_** @Documented annotation indicates that whenever the specified annotation is used those elements should be documented using the Javadoc tool.

**_@Target_** @Target annotation marks another annotation to restrict what kind of Java elements the annotation can be applied to. A target annotation specifies one of the following element types as its value:

1. ElementType.ANNOTATION_TYPE can be applied to an annotation type.
2. ElementType.CONSTRUCTOR can be applied to a constructor.
3. ElementType.FIELD can be applied to a field or property.
4. ElementType.LOCAL_VARIABLE can be applied to a local variable.
5. ElementType.METHOD can be applied to a method-level annotation.
6. ElementType.PACKAGE can be applied to a package declaration.
7. ElementType.PARAMETER can be applied to the parameters of a method.
8. ElementType.TYPE can be applied to any element of a class.

**_@Repeatable_** @Repeatable annotation indicates that the marked annotation can be applied more than once to the same declaration or type use.


### Generics 
Enable type to be a parameter while defining a class, method, interface etc.. 
Advantages : 

1. Compile time type checking 
2. Simple Readable code 
3. Generic algorithm 

Example: Write a method which sorts an Array, we wouldnt care what type of elemets are there in the array as long as the class implements Comparable class 
the above can be achieved by creating a bounded Type paramter.. 
    
    public <T extends Comparable<T>> void sort(T t){

    }

A comparable object is capable of comparing itself with another object. The class itself must implements the java.lang.Comparable interface to compare its instances. 

    class T implements Comparable<T>{
    	public int compareTo(T t){
		return this.getX().compareTo(t.getX());}
    }

Comparator is external to the element type we are comparing. It’s a separate class. Create multiple separate classes (that implement Comparator) to compare by different members. Collections method and it takes Comparator. The sort() method invokes the compare() to sort objects.

	class NameCompare implements Comparator<Movie> {
	    public int compare(T t1, T t2){
	        return t1.getX().compareTo(t2.getX());}
    	}

### MultiThreading
* by extending the Thread class(Callable class) : the call() method needs to be implemented which returns a result on completion. method can throw an exception 
* by creating a thread with a Runnable : cannot make a thread return result when it terminates. method cannot throw any exceptions


### Objects Basics 
1. **Cloning**
It is _faster_ than creation with the new keyword, because all the object memory is copied at once to the destination cloned object. Done by implementing *Cloneable* interface, which allows the method Object.clone() to perform a field-by-field copy.

2. **equals** method
It compares the object to another object and returns a boolean. 
compare the contents of the objects whereas the equality comparison operator "==" compares the object references. 

3. **finalize** method
It is called exactly once before the garbage collector frees the memory for object. 
A class overrides finalize to perform any clean up that must be performed before an object is reclaimed. 
If the JVM exits without performing garbage collection, the OS may free the objects, in which case the finalize method doesnt get called.

		protected void finalize() throws Throwable { ... }

4. **getClass** method
returns the java.lang.Class object for the class that was used to instantiate the object. 
The class object is the base class of reflection in Java. 

5. **hashCode** method
returns an integer (int). It is used by classes that provide associative arrays. 
It should be 
* Stable: does not change
* Evenly distributed
Util classes use : if a.hashCode() == b.hashCode() then a.equals(b)
In order to maintain this contract, a class that overrides the equals method must also override the hashCode method, and vice versa, so that hashCode is based on the same properties (or a subset of the properties) as equals.
base the hash function on immutable properties of the object

6. toString method
7. __wait and notify__ thread signaling methods
Every object has two wait lists for threads associated with it. One wait list is used by the synchronized keyword to acquire the mutex lock associated with the object. If the mutex lock is currently held by another thread, the current thread is added to the list of blocked threads waiting on the mutex lock. The other wait list is used for signaling between threads accomplished through the wait and notify and notifyAll methods.

Use of wait/notify allows efficient coordination of tasks between threads. When one thread needs to wait for another thread to complete an operation, or needs to wait until an event occurs, the thread can suspend its execution and wait to be notified when the event occurs. 

In *polling*,  the thread repeatedly sleeps for a short period of time and then checks a flag or other condition indicator. It is both more computationally expensive, as the thread has to continue checking, and less responsive since the thread won't notice the condition has changed until the next time to check.

The thread calling wait is blocked (removed from the set of executable threads) and added to the object's wait list. The thread remains in the object's wait list until one of three events occurs:

1. another thread calls the object's notify or notifyAll method;
2. another thread calls the thread's java.lang.Thread.interrupt method; or
3. a non-zero timeout that was specified in the call to wait expires.

The wait method must be called inside of a block or method synchronized on the object. This insures that there are no race conditions between wait and notify. When the thread is placed in the wait list, the thread releases the object's mutex lock. After the thread is removed from the wait list and added to the set of executable threads, it must acquire the object's mutex lock before continuing execution.

**notify and notifyAll methods**
The java.lang.Object.notify() and java.lang.Object.notifyAll() methods remove one or more threads from an object's wait list and add them to the set of executable threads. notify removes a single thread from the wait list, while notifyAll removes all threads from the wait list. Which thread is removed by notify is unspecified and dependent on the JVM implementation.

The notify methods must be called inside of a block or method synchronized on the object. This insures that there are no race conditions between wait and notify.

Arrays length in java cannot be changed 
Java can have jagged array.. 


Java doesnt support : default value in functions.. 
Java doesnt support operator overloading for functions.. 

#### Daemon thread 
Daemon thread is a low priority thread that runs in background to perform tasks such as garbage collection. This status can only be set when the threads are created. Usage is to provide services to user thread for background supporting task.

Properties:
* It is an utmost low priority thread.
* They can not prevent the JVM from exiting when all the user threads finish their execution.
* If JVM finds running daemon thread, it terminates the thread and after that shutdown itself. JVM does not care whether Daemon thread is running or not.
__void setDaemon(boolean status)__: This method is used to mark the current thread as daemon thread or user thread. 

#### Reflection 
API used to modify behaviour of methods, classes, interfaces at runtime.
Reflection gives us information about the class to which an object belongs and also the methods of that class which can be executed by using the object. 
Can invoke methods at runtime irrespective of the access specifier used with them.
* import java.lang.reflect package.
* __Class__ getClass(): to get the name of the class to which an object belongs.
* __Constructors__ getConstructors() : to get the public constructors of the class to which an object belongs.
* __Methods__ getMethods(): to get the public methods of the class to which an objects belongs.

Advantages of Using Reflection:
* __Extensibility Features__ An application may make use of external, user-defined classes by creating instances of extensibility objects using their fully-qualified names.
* __Debugging and testing tools__ Debuggers use the property of reflection to examine private members on classes.

Drawbacks:
* __Performance Overhead__ Reflective operations have slower performance than their non-reflective counterparts, and should be avoided in sections of code which are called frequently in performance-sensitive applications.
* __Exposure of Internals__ Reflective code breaks abstractions and therefore may change behavior with upgrades of the platform.

### Thread Pools 
A thread pool reuses previously created threads to execute current tasks and offers a solution to the problem of thread cycle overhead and resource thrashing. Since the thread is already existing when the request arrives, the delay introduced by thread creation is eliminated, making the application more responsive.

Java provides the Executor framework which is centered around the Executor interface, its sub-interface 
* sub-interface –ExecutorService 
* class-ThreadPoolExecutor, which implements both of these interfaces. 
By using the executor, one only has to implement the Runnable objects and send them to the executor to execute.

They allow you to take advantage of threading, but focus on the tasks that you want the thread to perform, instead of thread mechanics.
To use thread pools, we first create a object of ExecutorService and pass a set of tasks to it. ThreadPoolExecutor class allows to set the core and maximum pool size.The runnables that are run by a particular thread are executed sequentially.

* newFixedThreadPool(int)           Creates a fixed size thread pool.
* newCachedThreadPool()             Creates a thread pool that creates new  threads as needed, but will reuse previously constructed threads when they are available
* newSingleThreadExecutor()         Creates a single thread. 

       public class Test {
	     // Maximum number of threads in thread pool
	    static final int MAX_T = 3;             
	    public static void main(String[] args) {
		Runnable r1 = new Task("task 1");
                ExecutorService pool = Executors.newFixedThreadPool(MAX_T);  
	        // passes the Task objects to the pool to execute (Step 3)
        	pool.execute(r1);
	        // pool shutdown ( Step 4)
	        pool.shutdown();    
    		}
      }

Risks in using Thread Pools
* __Deadlock__ Thread pools introduce another case of deadlock, one in which all the executing threads are waiting for the results from the blocked threads waiting in the queue due to the unavailability of threads for execution.
* __Thread Leakage__ Thread Leakage occurs if a thread is removed from the pool to execute a task but not returned to it when the task completed. As an example, if the thread throws an exception and pool class does not catch this exception. 
* __Resource Thrashing__ If the thread pool size is very large then time is wasted in context switching between threads. 


#### Runtime
* __JVM Shutdown Hook__ Construct that allow developers to plug in a piece of code to be executed when the JVM is shutting down. This comes in handy in cases where we need to do special clean up operations in case the VM is shutting down.
A Class which extends the java.lang.Thread class, and provide the logic that we want to perform when the VM is shutting down, inside the public void run() method. Then we register an instance of this class as a shutdown hook to the VM by calling Runtime.getRuntime().addShutdownHook(Thread) method.

* __public void loadLibrary(String libname)__ This method loads the dynamic library with the specified library name. A file containing code is loaded from the local system from a place where library files are conventionally obtained.

* __public void gc()__ This method runs the garbage collector. Calling this method suggests that the Java virtual machine expend effort toward recycling unused objects in order to make the memory they currently occupy available for quick reuse.


#### Grabage Collection 
__Mark Phase__
When an object is created, its mark bit is set to 0(false). In the Mark phase, we set the marked bit for all the reachable objects (or the objects which a user can refer to) to 1(true). 
Now to perform this operation we simply need to do a graph traversal, a depth first search approach would work for us. 
Root is a variable that refer to an object and is directly accessible by local variable. 
Note: If we have more than one root, then we simply have to call Mark() for all the root variables.

__Sweep Phase__
As the name suggests it “sweeps” the unreachable objects i.e. it clears the heap memory for all the unreachable objects. 
All those objects whose marked value is set to false are cleared from the heap memory, for all other objects (reachable objects) the marked bit is set to false.

__Disadvantages of Mark and Sweep Algorithm__
* Normal program execution is suspended while the garbage collection algorithm runs.
* It is run several times on a program, reachable objects end up being separated by many, small unused memory regions. Look at the below figure for better understanding.
    
__Advantages of Mark and Sweep Algorithm__
* It handles the case with cyclic references, even in case of a cycle, this algorithm never ends up in an infinite loop.
* There are no additional overheads incurred during the execution of the algorithm.


#### Producer Consumer 

    LinkedList list 
    public class Producer implements Runnable{
	    public class run(){
	    }
    }
    public class Consumer implements Runnable{
	    public class consume(){
	    }
    }
    }
    
### Deadlock 
__Semaphore__
The thread that wants access to the shared resource tries to acquire a permit.
If the semaphore’s count is greater than zero, then the thread acquires a permit, which causes the semaphore’s count to be decremented.
Otherwise, the thread will be blocked until a permit can be acquired.
If there is another thread waiting for a permit, then that thread will acquire a permit at that time.

We can use a semaphore to lock access to a resource, each thread that wants to use that resource must first call acquire( ) before accessing the resource to acquire the lock. When the thread is done with the resource, it must call release( ) to release lock

__synchronized__ keyword is used to make the class or method thread-safe which means only one thread can have lock of synchronized method and use it, other threads have to wait till the lock releases and anyone of them acquire that lock.

### Serialization
__java.io.Serializable__ interface. It doesnt have any methods or variables, its just a __marker interface__. 
_Static_ and _transient_ variables are not serialized. 
__transient__ keyword : to mark variables which should not be serialized. One may have fields that are derived from other fields, and should only be done so programmatically, rather than having the state be persisted via serialization. 

__SerialVersionUID__ is associated with each Serializable class. it is used during Deserialization to verify that sender and reciever of a serialized object have loaded classes for that object which are compatible with respect to serialization. If the reciever has loaded a class for the object that has different UID than that of corresponding sender’s class, the Deserialization will result in an __InvalidClassException__.A Serializable class can declare its own UID explicitly by declaring a field name.


### Misc
__java.lang.Cloneable__ override clone() method from Object class. Syntax of the clone() method is :

    protected Object clone() throws CloneNotSupportedException

__TreeMap__
TreeMap is a ordered collection of the keys. Uses a red-black tree in the background, to ensure no duplicates. For operations like add, remove, containsKey, time complexity is O(log n). TreeMap implements SortedMap interface
     
     static String electionWinner(String[] votes) {
        Map<String, Integer> voteCount = new TreeMap<>(Collections.reverseOrder()); //By deault its asceding ordering in keys 
        for(String candidate : votes){
            voteCount.put(candidate, voteCount.getOrDefault(candidate, 0) + 1);
        }
        return Collections.max(voteCount.entrySet(), Map.Entry.comparingByValue()).getKey();
    }
     

