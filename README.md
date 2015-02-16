#Scala_basics

#Fundamentals
###1. Scala interpreter
<b>1. Get into scala</b><br>
Set environment parameter:
```java
export SCALA_HOME=[/..../scala-2.11.5 ]
export PATH=${PATH}:${SCALA_HOME}/bin
```
In shell, type in:
```java
$ scala
```
You'll get:
```java
scala>
```
<b>2. Function</b><br>
Define max function:
```java
scala> def max(x: Int, y: Int) = if (x>y) x else y
```
to call the function:
```java
scala>max(2,3)
```
<b>3. Scripts</b><br>
put the code in hello.scala
```java
println("hello " + args(0) + "!")
```
to run it:
```java
$ scala hello.scala world
```
###2. Values and Variables
values are immutable
```java
scala> val answer = 8
scala> answer = 0
error: reassignment to val
```
variables are mutable
```java
scala> var counter = 0
scala> counter = 1
```
types of values and variables can be inferred by the assignments, but it can be specified explicitly
```java
val greeting: String = null
var greeting: Any = "Hello"
```
###3. Types
same with 8 primitive types in Java, no discrimination between class and primitive types, no wrapper class
```java
1.toString() // returns a String "1"
1.to(10) // generates a range from 1 to 10 inclusively
```
###4. Arithmetical operators
```java
val answer = a + b	 // + is a function name
val answer = a.+(b) // + is a function name
```
No ++, -- self increment/decrement
###5. apply()
```java
"hello".apply(4) // char 'o'
```
```java
def bigPow(x: Int, y: Int): BigInt = if (y == 1) BigInt.apply(x) else bigPow(x, y-1)*x
```

#Statement control

###1. If/else statements
if statements have a value
```java
val s = if (x>0) 1 else -1	// better, cause s is a value
if (x > 0) s = 1 else s = -1 // s must be  var
```

###2. Loop statement
whlie loop
```java
while (n > 0) {
	r = r * n
	n -= 1
}
```
for loop
```java
val s = "Hello"
var sum = 0
for (i <- 1 to s.length-1)	// i from 1 to s.length-1 inclusively
	sum += s(i)
```

```java
val s = "Hello"
var sum = 0
for (i <- 1 until s.length)	// i from 1 to s.length-1 inclusively
	sum += s(i)
```

```java
val s = "Hello"
var sum = 0
for (ch <- "Hello") sum += ch
```
Advanced for loop
```java
for (i <- 1 to 3; j <- -1 to 3) print ((10*i+j) + " ")
	// 11 12 13 21 22 23 31 32 33
```
guard:
```java
for (i <- 1 to 3; j <- -1 to 3 if i != j && i >= 2) print ((10*i+j)) + " ")
	// 21 23 31 32
```
for/yield
```java
for (i <- 1 to 10) yield i % 3	// generates an immutable vector (1,2,0,1,2,0,1,2,0,1)
```
###3. Function
iterative fibo
```java
def fac(n: Int) = {var r = 1; for (i <- 1 to n) r = r * i; r}
```
recursive function (return type must be specified)
```java
def fac(n: Int) : Int = if (n <= 0) 1 else n * fac(n-1)
```
default parameter
```java
def decorate(str:String, left:String = "[", right:String = "]") = left + str + right
decorate("hello")	// "[hello]"
decorate("hello", "<<", ">>")	//"<<hello>>"
```
variable-length parameters
```java
def sum(args: Int*) = {
	var result = 0
	for (arg <- args) result += arg
	result
}
```
###4.Lazy value
when a value is decalred lazy, its initialization will be postponed till first access
```java
lazy val words = scala.io.Source.fromFile("/invalid/path").mkString
// when words is accessed, FileNotFound
```
#Array
###1. Fixed-length array
```java
val nums = new Array[Int](10)		// an array of 10 0's
val strs = new Array[String](10)	// an array of 10 null strings
val s = Array("Hello", "World")		// type inference
```
###2. Variable-length array
counterpart of ArrayList in Java
```java
val b = ArrayBuffer[Int]()
b += 1								// (1)
b += (1,2,3,5)						// (1,1,2,3,5)
b.append(1,2,3,5)
b ++= Array(0,1)					// (1,1,2,3,5,0,1)
b.appendAll(Array(0,1))
b.trimEnd(5)						// (1,1)
b.insert(1,6)						// (1,6,1)
b.remove(1)							// (1,1)
b.remove(0,2)						// ()
```
#Map and tuple
###1. Create a map
create an immutable Map[String, Int]
```java
import scala.collection.mutable.Map
val scores = Map("Alice" -> 10, "Bob" -> 3, "Cindy" -> 8)
```
create a map from empty
```java
import scala.collection.mutable.HashMap
val scores = new HashMap[String, Int]
for (i <- 1 to 10)
	scores += (i.toString -> i)
``` 
###2. Operations
get value
```java
val firstScore = scores("1")
```
check contained
```java
scores.contains("1")
```
shortcut
```java
val fristScore = scores.getOrElse("1", 0)	// if not contained, return 0
```
update value
```java
scores("1") = 0
scores += ("10"->10, "11"->11)
```
remove an entry
```java
scores -= "2"
```
keySet and valueSet
```java
scores.values.foreach {println}
scores.keySet.foreach {println}
```
zipped pairs
```java
val symbols = Array("<", "-", ">")
val counts = Array(2, 10, 2)
val pairs = symbols.zip(counts)			// Array(("<", 2), ("-", 10), (">", 2))
```
#Class
###Simple class
define a class
```java
class Counter {
	private var value = 0				// must be initialized
	def increment() { value += 1 }		// public by default
	def current() = value
}
```
invoke funcitons
```java
val myCounter = new Counter				// or new Counter()
myCounter.increment()
println(myCounter.current)				// or myCounter.current()
```
###Getter and Setter
an object of a class implements the setter and getter by default
```java
class Person {
	var age = 0
}
```
```java
val p = new Person
p.age									// res: Int = 0
p.age_=(1)								// setter
```
###Constructor
```java
class Person {
	private var name = ""
	private var age = 0
	
	def this(name: String) {
		this()
		this.name = name
	}
	
	def this(name: String, age: Int) {
		this(name)
		this.age = agexdf
	}
}
```
main constructor
```java
class Person () {
	println("constructed")		// executed when constructed
}
```
#Object
###Singleton object
object can act as static fields or functions
```java
object Accounts {
	private var lastNumber = 0
	def newUniqueNumber() = {lastNumber += 1; lastNumber}
}
```

```java
Accounts.newUniqueNumber
```
###Companion object
when defining a class, you can create a companion object to hold static functions (In REPL, :paste to enter paste mode, Ctrl+D to exit)
```java
class Account {
	val id = Account.newUniqueNumber()
	private var balance = 0.0
	def deposit(amount: Double) {balance += amount}
}

object Account {
	private var lastNumber = 0
	private def newUniqueNumber() = {lastNumber += 1; lastNumber}
}
```
#High order function
###Function as a value
```java
import scala.math._
val num = 3.14
val fun = ceil _		// _ indicates ceil is function
```
```java
fun(num)				// 4.0
```
map function: takes a function, called by Array of parameters, forearmsch is the same, but no return values
```java
Array(3.14, 1.42, 2.0).map(fun)		// Array(4.0, 2.0, 2.0)
```
anonymous function: (parameters) => operations
```java
(x: Double) => 3 * x
Array(3.14, 1.42, 2.0).map((x:Double) => 3 * x)
```
###Function as a parameter
```java
def valueAt0.25(f: (Double) => Double) = f(0.25)
```
```java
valueAt0.25(ceil _)					// 1.0
valueAt0.25(sqrt _)					// 0.5
```
###Type inference
```java
valueAt0.25 (x => 3 * x)			// 0.75
valueAt0.25 (3 * _)					// only applies here
```
###Useful high order function
map, foreach, filter: takes a function returning Bool
```java
(1 to 9).filter(_ % 2 == 0)			// 2, 4, 6, 8
```
###Closure
```java
def mulBy(factor: Double) = (x: Double) => factor * x
```
every returned function has its own factor setting
```java
val triple = mulBy(3)
val half = mulBy(0.5)
triple(14) + half(14)
```

###Curry
change a function taking two parameters into a function taking one
```java
def mul(x: Int, y: Int) = x * y
def mulOnce(x: Int) = (y: Int) => x * y
def mulOnceScala(x: Int) (y: Int) = x * y
```
```java
mul(5, 2)				// res: Int = 10
mulOnce(5)(2)			// res: Int = 10
```