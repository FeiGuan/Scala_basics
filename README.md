#Scala_basics

#Fundamentals
###1. Scala interpreter
<b>1. Get into scala</b><br>
Set environment parameter:
<pre>
export SCALA_HOME=[/..../scala-2.11.5 ]
export PATH=${PATH}:${SCALA_HOME}/bin
</pre>
In shell, type in:
<pre>
$ scala
</pre>
You'll get:
<pre>
scala>
</pre>
<b>2. Function</b><br>
Define max function:
<pre>
scala> def max(x: Int, y: Int) = if (x>y) x else y
</pre>
to call the function:
<pre>
scala>max(2,3)
</pre>
<b>3. Scripts</b><br>
put the code in hello.scala
<pre>
println("hello " + args(0) + "!")
</pre>
to run it:
<pre>
$ scala hello.scala world
</pre>
###2. Values and Variables
values are immutable
<pre>
scala> val answer = 8
scala> answer = 0
error: reassignment to val
</pre>
variables are mutable
<pre>
scala> var counter = 0
scala> counter = 1
</pre>
types of values and variables can be inferred by the assignments, but it can be specified explicitly
<pre>
val greeting: String = null
var greeting: Any = "Hello"
</pre>
###3. Types
same with 8 primitive types in Java, no discrimination between class and primitive types, no wrapper class
<pre>
1.toString() // returns a String "1"
1.to(10) // generates a range from 1 to 10 inclusively
</pre>
###4. Arithmetical operators
<pre>
val answer = a + b	 // + is a function name
val answer = a.+(b) // + is a function name
</pre>
No ++, -- self increment/decrement
###5. apply()
<pre>
"hello".apply(4) // char 'o'
</pre>
<pre>
def bigPow(x: Int, y: Int): BigInt = if (y == 1) BigInt.apply(x) else bigPow(x, y-1)*x
</pre>

#Statement control

###1. If/else statements
if statements have a value
<pre>
val s = if (x>0) 1 else -1	// better, cause s is a value
if (x > 0) s = 1 else s = -1 // s must be  var
</pre>

###2. Loop statement
whlie loop
<pre>
while (n > 0) {
	r = r * n
	n -= 1
}
</pre>
for loop
<pre>
val s = "Hello"
var sum = 0
for (i <- 1 to s.length-1)	// i from 1 to s.length-1 inclusively
	sum += s(i)
</pre>

<pre>
val s = "Hello"
var sum = 0
for (i <- 1 until s.length)	// i from 1 to s.length-1 inclusively
	sum += s(i)
</pre>

<pre>
val s = "Hello"
var sum = 0
for (ch <- "Hello") sum += ch
</pre>
Advanced for loop
<pre>
for (i <- 1 to 3; j <- -1 to 3) print ((10*i+j) + " ")
	// 11 12 13 21 22 23 31 32 33
</pre>
guard:
<pre>
for (i <- 1 to 3; j <- -1 to 3 if i != j && i >= 2) print ((10*i+j)) + " ")
	// 21 23 31 32
</pre>
for/yield
<pre>
for (i <- 1 to 10) yield i % 3	// generates an immutable vector (1,2,0,1,2,0,1,2,0,1)
</pre>
###3. Function
iterative fibo
<pre>
def fac(n: Int) = {var r = 1; for (i <- 1 to n) r = r * i; r}
</pre>
recursive function (return type must be specified)
<pre>
def fac(n: Int) : Int = if (n <= 0) 1 else n * fac(n-1)
</pre>
default parameter
<pre>
def decorate(str:String, left:String = "[", right:String = "]") = left + str + right
decorate("hello")	// "[hello]"
decorate("hello", "<<", ">>")	//"<<hello>>"
</pre>
variable-length parameters
<pre>
def sum(args: Int*) = {
	var result = 0
	for (arg <- args) result += arg
	result
}
</pre>
###4.Lazy value
when a value is decalred lazy, its initialization will be postponed till first access
<pre>
lazy val words = scala.io.Source.fromFile("/invalid/path").mkString
// when words is accessed, FileNotFound
</pre>
#Array
###1. Fixed-length array
<pre>
val nums = new Array[Int](10)		// an array of 10 0's
val strs = new Array[String](10)	// an array of 10 null strings
val s = Array("Hello", "World")		// type inference
</pre>
###2. Variable-length array
counterpart of ArrayList in Java
<pre>
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
</pre>
#Map and tuple
###1. Create a map
create an immutable Map[String, Int]
<pre>
import scala.collection.mutable.Map
val scores = Map("Alice" -> 10, "Bob" -> 3, "Cindy" -> 8)
</pre>
create a map from empty
<pre>
import scala.collection.mutable.HashMap
val scores = new HashMap[String, Int]
for (i <- 1 to 10)
	scores += (i.toString -> i)
</pre> 
###2. Operations
get value
<pre>
val firstScore = scores("1")
</pre>
check contained
<pre>
scores.contains("1")
</pre>
shortcut
<pre>
val fristScore = scores.getOrElse("1", 0)	// if not contained, return 0
</pre>
update value
<pre>
scores("1") = 0
scores += ("10"->10, "11"->11)
</pre>
remove an entry
<pre>
scores -= "2"
</pre>
keySet and valueSet
<pre>
scores.values.foreach {println}
scores.keySet.foreach {println}
</pre>
zipped pairs
<pre>
val symbols = Array("<", "-", ">")
val counts = Array(2, 10, 2)
val pairs = symbols.zip(counts)			// Array(("<", 2), ("-", 10), (">", 2))
</pre>
#Class
###Simple class
define a class
<pre>
class Counter {
	private var value = 0				// must be initialized
	def increment() { value += 1 }		// public by default
	def current() = value
}
</pre>
invoke funcitons
<pre>
val myCounter = new Counter				// or new Counter()
myCounter.increment()
println(myCounter.current)				// or myCounter.current()
</pre>
###Getter and Setter
an object of a class implements the setter and getter by default
<pre>
class Person {
	var age = 0
}
</pre>
<pre>
val p = new Person
p.age									// res: Int = 0
p.age_=(1)								// setter
</pre>
###Constructor
<pre>
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
</pre>
main constructor
<pre>
class Person () {
	println("constructed")		// executed when constructed
}
</pre>
#Object
###Singleton object
object can act as static fields or functions
<pre>
object Accounts {
	private var lastNumber = 0
	def newUniqueNumber() = {lastNumber += 1; lastNumber}
}
</pre>

<pre>
Accounts.newUniqueNumber
</pre>
###Companion object
when defining a class, you can create a companion object to hold static functions (In REPL, :paste to enter paste mode, Ctrl+D to exit)
<pre>
class Account {
	val id = Account.newUniqueNumber()
	private var balance = 0.0
	def deposit(amount: Double) {balance += amount}
}

object Account {
	private var lastNumber = 0
	private def newUniqueNumber() = {lastNumber += 1; lastNumber}
}
</pre>
#High order function
###Function as a value
<pre>
import scala.math._
val num = 3.14
val fun = ceil _		// _ indicates ceil is function
</pre>
<pre>
fun(num)				// 4.0
</pre>
map function: takes a function, called by Array of parameters, forearmsch is the same, but no return values
<pre>
Array(3.14, 1.42, 2.0).map(fun)		// Array(4.0, 2.0, 2.0)
</pre>
anonymous function: (parameters) => operations
<pre>
(x: Double) => 3 * x
Array(3.14, 1.42, 2.0).map((x:Double) => 3 * x)
</pre>
###Function as a parameter
<pre>
def valueAt0.25(f: (Double) => Double) = f(0.25)
</pre>
<pre>
valueAt0.25(ceil _)					// 1.0
valueAt0.25(sqrt _)					// 0.5
</pre>
###Type inference
<pre>
valueAt0.25 (x => 3 * x)			// 0.75
valueAt0.25 (3 * _)					// only applies here
</pre>
###Useful high order function
map, foreach, filter: takes a function returning Bool
<pre>
(1 to 9).filter(_ % 2 == 0)			// 2, 4, 6, 8
</pre>
###Closure
<pre>
def mulBy(factor: Double) = (x: Double) => factor * x
</pre>
every returned function has its own factor setting
<pre>
val triple = mulBy(3)
val half = mulBy(0.5)
triple(14) + half(14)
</pre>

###Curry
change a function taking two parameters into a function taking one
<pre>
def mul(x: Int, y: Int) = x * y
def mulOnce(x: Int) = (y: Int) => x * y
def mulOnceScala(x: Int) (y: Int) = x * y
</pre>
<pre>
mul(5, 2)				// res: Int = 10
mulOnce(5)(2)			// res: Int = 10
</pre>