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
7. wait and notify thread signaling methods
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



    
