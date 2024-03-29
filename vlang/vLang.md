# V Programming

My learning notes with the book ""Getting Started with V Programming""

- [Preface](#preface)
- [Introduction](#chapter-1-introduction)
- [Installation](#chapter-2-installation)
- [Variables](#chapter-3-variables)
- [Primitives Data Types](#chapter-4-primitives-data-types)
- [Primitives Data Structures](#chapter-5-data-structures)
- [Flow Control](#chapter-6-flow-control)
- [Functions](#chapter-7-functions)
- [Structures](#chapter-8-structs)
- [Modules](#chapter-9-modules)
- [Concurrency](#chapter-10-concurrency)
- [Channels](#chapter-11-channels)
- [Testing](#chapter-12-testing)

## Preface

- You can participate in [discussions](https://github.com/vlang/v/discussions) on [Discord](https://discord.gg/vlang).
- This note would be helpful for someone needs to know what Vlang is, or someone needs a memory refreshing on the main topics or a reference for the syntax

## Chapter 1: Introduction

- What is Vlang?

  - Simple, fast, safe, compiled and statically typed programming language.

- What makes Vlang awesome?

  - Performance like C.
  - Simplicity & maintainability syntax like Go.
  - Safety (immutability, no null, option types, free from data races) like Rust.
    Having null values in a programming language enforces you to handle the null scenarios using multiple checks. These checks, when missed, might lead to errors.
  - Memory management using the autofree engine.
  - Friendly wih C as backend and the code can be converted to JS
  - Alot of Built-in libs like ORM `orm`, web server `vweb` and GUI `ui`.

## Chapter 2: Installation

```bash
sudo apt -y update
sudo apt install -y build-essential
git clone https://github.com/vlang/v
cd v
make
sudo ./v symlink
v
```

## Chapter 3: Variables

- Variables is V are immutable by default use `mut` keyword instead to make it mutable.
- The symbol`:=` is used for deceleration and `=` for reassignment.
- V also support
  - Parallel variable declaration `a, b := 1, 3`
  - Augmented assignment `cnt += 5`
- The mutation only works for the values of the same types.
- Variable rules:

  - V requires you to define a variable with an initial value. This is because V tries to avoid having null values
  - All the variables must be consumed once declared
  - no global variables: The variable declared in the conditionals and iterators or functions is only scoped to the respective to it.
    - but can be enabled with compiler flag `-enable-globals` for low level applications
  - Variable shadowing is not allowed in V: same variable name can hold different values in the outer and inner scope.

- Constants have a larger scope not for a specific block but for the file (aka: module level). and it must be defined at a module level in a `const` block

  ```v
  const {
    pi = 3.14
  }
  ```

| Variable            | Constants       |
| ------------------- | --------------- |
| declared using `:=` | using `=`       |
| at function level   | at module level |

## chapter 4: Primitives data types

- As most languages V support the data types (boolean, numeric (all variations), string) plus runes
- Discussing each one and the related operations:
- Boolean:
  - `true`, `false`.
  - What causes boolean values?
    - literal: `completed := true`
    - logical expressions(&&, ||, !): `allowed := completed && passed`
    - relational expressions(<, >, <=, >=, ==): `allowed := age >= 30`
- Numeric:
  - signed int: `i8`, `i16`, `int`: i32, `i64` and unsigned int: `u8`: byte, `u16`, `u32`, `u64` and float: `f32` or `f64`
  - binary: `0b0001001`, octal `0o461`, decimal `17173710`, hexadecimal `0xa17e`
  - Operations on numbers:
    - arithmetic `+-*/%`
    - bitwise (logic gates) `&|^~`
    - shifting `<< >>`
      - The shift operator acts on the bit allocations of the integer and thereby moves the bits left or right based on the << and >> operators, respectively, filling the shifted positions with 0.
- Strings:

  - the value held by the variable is enclosed in single quotes (') or doubled
  - A string in V is implemented using a struct type that has two fields, str and len, marked as public using the pub keyword. The str field is a byte pointer with a default value of 0 assigned to it. The len field is of the int type, representing the length of a string.
  - the elements of a string can be indexed but it result the value of the byte

    ```v
    mut name := 'Omar'
    println(name[0])
    ```

  - update the value of a character in a string at a certain position. This is not allowed for strings even if they are declared as mutable because strings are read-only arrays of bytes

    ```v
    mut name := 'Omar'
    name[0] = 'U' // not allowed
    ```

  - Operations:

    - interpolation

      ```v
      println('Hello, $name')
      ```

    - concatenation

      ```v
      age := 24
      println('Hello ' + name)
      println('$name is $age years old')
      ```

    - slicing

      ```v
      a :='Camel'
      b := a.substr(0,3)
      println(b) // Cam
      ```

    - spliting

      ```v
      sp :='The tiny tiger tied the tie tighter to its tail'
      res := sp.split(' ') // split by space as delimiter
      println(res) // // ['The', 'tiny', 'tiger', 'tied', 'the', //'tie', 'tighter', 'to', 'its', 'tail']

      ```

    - counting

      ```v
      sp := 'The tiny tiger tied the tie tighter to its tail'
      println(sp.count('t'))
      ```

    - membership

      ```v
      hs := 'monday'
      if hs.contains('mon') {
        println('$hs contains mon')
      }
      ```

- Runes:
  - Briefly put, character literals have a specific data type called a rune. A rune represents a Unicode code point. The rune type is an alias for the unsigned integer u32 in V
  - from string:
    ```v
    doge_moon := '🐶+🚀=🌚'
    doge_moon_runes := doge_moon.runes()
    println(doge_moon_runes)
    ```
- useful commands `typeof().name` and `sizeof()`

## Chapter 5: Data Structures

- V also has the primitives data structures like the other languages (arrays and maps)
- Arrays:

  - Deceleration

    ```v
    // empty
    mut arr := []string{}

    // schema
    mut arr1 := []int {len: 3, init: 1}

    // fixed
    mut arr2 := [4]int{}

    // init
    mut arr3 := ['Omar', 'abdulaziz']

    // matrix
    mut arr4 := [][]int{ len:4 , init: []int{len: 2}}
    mut arr5 := [[0,1], [0, 0], [1,0], [1,1]]
    ```

  - Accessing
    ```v
    mut sports := ['cricket', 'hockey', 'volleyball', 'baseball']
    println(sports[1])
    ```
  - Slicing
    ```v
    println(sports[1..sports.len-1])
    println(sports[1..])
    ```
  - membership
    ```v
    odd := [1, 3, 5, 7]
    println(3 in odd)
    ```
  - appending
    ```v
    mut even := [2, 4, 6]
    even << 8
    ```
  - cloning
    ```v
    r := [1,2,3,4]
    mut u := r.clone()
    ```
  - sorting
    The sort function optionally accepts a sort expression as an input argument.Generally, the sort expression indicates the order in which the elements need to be sorted. The sort expression is built using two special a and b variables and one of the > or < operators
    ```v
    mut i := [ 3, 2, 8, 1]
    i.sort() // == i.sort(a < b)  ascending order
    println(i)
    i.sort(a > b) //descending order
    println(i)
    ```
    The a, b comes in handy when the member of the array is an instances from structs
    ```v
    students.sort(a.id > b.id)
    ```
  - filtering
    The filter function accepts a filter condition projected on each element of the array. The condition comprises an expression with the built-in it variable, which is used to represent the element that is being evaluated against the condition followed by the filter condition. The filter results in a new partial array based on the filter condition and it does not act on the original array where the filter is applied.
    ```v
    f := [1, 2, 3, 4, 5, 6, 7, 8, 9]
    multiples_of_3 := f.filter(it % 3 == 0)
    println(multiples_of_3)
    ```
  - mapping
    Applying mapping onto array elements V allows you to map each of the elements of any array in order to produce a new array in another form. Unlike the filter function, which accepts a filter condition and only acts on the array elements that satisfy the filter condition, a map applies to all the elements of an array. The map function is available on an array variable that accepts the operation to be performed on each element of the array as an input argument. Each element of an array during the map operation is referenced by the built-in it variable.
    ```v
    visitor := ['Tom', 'Ram', 'Rao']
    res := visitor.map('Mr. $it')
    println(res)
    ```

- Maps:

  - Declare empty

    ```v
    mut books := map[string]int{}
    ```

  - or init

    ```v
    mut student_1 := {
    'english' : 90,
    'mathematics' : 96,
    'physics' : 83,
    'chemistry' : 89
    }
    ```

  - Counting: You can check the number of key-value pairs present in a map using len, which is a read-only property available on the map variable.
    ```v
    cnt := student_1.len
    ```
  - CRUD
    - Create
      ```v
      books['V on Wheels'] = 320
      ```
    - Read
      ```v
      println(student_1['physics'])
      ```
    - Update
      ```v
      student_1['english'] = 93
      ```
    - Delete
      ```v
      student_1.delete('physics')
      ```
  - OR
    Let's consider the scenario when we try to access non-existing element instead of returning 0, which is done by V, we can use the `or {}` block to show a detailed error message:
    ```v
    res2 := student_1['geography'] or { panic('marks for subject $sub not yet updated')} // throws error
    ```

## Chapter 6: Flow Control

As the rest of all programming langs you can make a conditional or Iterative statements in V.

- Conditional

  - if
    ```v
    if CONDITION_1 {
      CONDITION_1 is ture
    } else if CONDITION_2 {
      CONDITION_2 is ture
    } else {
      all false
    }
    ```
  - goto
    go to defined label, A goto statement must be wrapped inside an unsafe block. This is because goto allows the program execution flow to move past variable initialization or return to code that accesses memory that has already been freed. As the goto statement requires an unsafe block, it should be avoided to prevent the risk of violating memory safety
    ```v
    sample_label:
          println('this will be called when goto is invoked')
    unsafe {
        goto sample_label
    }
    ```
  - match
    known in other languages as switch and it is a better way to make a chaining if else-if
    ```v
    match day {
      'Sun', 'Mon' { println('welcome new week')}
      'Fri', 'Sat' { println('weekend')}
      else { println('an other day')}
    }
    ```
    this is good but the actual power of match can be leveraged for pattern matching.
    ```v
    age := 18
    res := match age {
      0...18 { 'Person with age $age classified as a Child' }
      19...120 { 'Person with age $age classified as an Adult' }
      else { '$age must be in the range 0 to 120' }
    }
    println(res)
    ```
  - Notice the `...` is the range operator.

- Iterative
  There is a lot of different styles and all done with `for`, if you only need one of the iterative vars you can ignore the other with `_`

  - for
    ```v
    for KEY_VAR, VALUE_VAR in MAP_VAR {}
    for _, VALUE_VAR in MAP_VAR {}
    for INDEX_VAR, VALUE_VAR in ARRAY_VAR {}
    for VALUE_VAR in ARRAY_VAR {}
    for INIT; CONDITION; CHANGE {} // C-style loop
    for VAL in RANGE {} // for i in 0..5
    for {}
    ```
  - `break` to exit the loop and `continue` to skip the current iteration.

## Chapter 7: Functions

The art of wrapping or grouping a logically related set of statements, providing it with a name, and optionally providing input arguments and return types
The full structure looks like:

```v
ACCESS-MODIFIER fn FUNCTION_NAME(ARGUMENT1_NAME ARGUMENT1_ DATATYPE, ARGUMENT2_NAME ARGUMENT2_DATATYPE) RETURN_DATATYPE {
  OPERATIONS
}
```

- Function types

  - the main function
    - the entrypoint for the v program, accept no argument and return none
    - needs to be placed in a file at the root of the directory structure. and the file should be specified with `module main`
  - the basic function
    ```v
    fn greet(name string) string {
      return "Hello, $name"
    }
    println(greet("Omar"))
    ```
  - the anonymous function
    - don't have any name and can be created inside another function
    - can be declared and assigned to a variable on the fly such that they can be invoked by calling that variable
    - The scope of anonymous functions is within the scope of the function where they are declared
    ```v
    greet2 := fn(name string) string {
      return "Hello, $name"
    }
    ```
  - the higher order function
    A function that accept or return another function

    - A function that accept other function you need to specify the type of the lower function as `fn(arg_type) return_type`

      ```v
      fn higher_fn ( lower_fn fn () string, name string) string {
        // ...
      }
      ```

    - A function that return other function
      ```v
      fn do_operation ( op Operations ) fn (int, int) int {
        // ...
      }
      ```

- Function features

  - functions in V are pure. This means that return values are a function of their arguments only
  - function can accept multiple args and return multiple vals `fn(a int, b int) (int, int) {}`
  - Ignoring return values from functions that return multiple values with `_`
  - Functions allow only arrays, interfaces, maps, pointers, and structs as mutable arguments
    means that the function can be not pure and mutate the value of the passed argument and that by putting `mut` keyword in the fn signature
    ```v
    fn inc( mut arr []int, num int) {
      for mut i in arr {
        i+= num
      }
    }
    a := [1,3]
    println(inc(a, 2)) // [3,5]
    ```
  - Function declarations in script mode should come before all script statements `.vsh`
  - Functions do not allow access to module variables or global variables
    that is because the function is pure by default and only can access the variables passed as arguments but that can be altered by `_global {}` scope and `-enable-globals` compiler flag
  - Functions do not allow default or optional arguments
    V does not allow you to declare functions with its arguments set to default values. This indicates that V does not support optional arguments. However, it is interesting to know that structs in V can be defined with default values assigned to their fields. To achieve this functionality, you can overcome this limitation by creating a function that accepts a struct as an argument
  - Functions can have optional return types achived by `?` operator before the return type and the caller of the funciton must specify `or {}` block

    ```v
    fn is_age(age int) ?string {
      if age < 0 {
        return error('invalid age provided')
      } else {
        return 'live long you'
      }
    }

    println(is_age(-3) or { err.msg })
    ```

  - Functions are private by default and can be exposed to other modules using the pub access modifier keyword
  - Functions allow you to defer the execution flow using the defer block `defer {}`
  - Functions can be represented as elements of an array or a map only if the functions have the same signature
    ```v
    funcs := [adder, subtractor, multiplier]
    i, j := 2, 5
    for f in funcs {
      res := f(i, j)
      println(res)
    }
    ```

## Chapter 8: Structs

struct is a schema for defining a type, it has no values just the types of the elements it contains

- Define
  ```v
  struct Note {
    id int
    content string
  }
  ```
- Init

  ```v
  note := Note {
    id: 12
    content: "Hi"
  }
  ```

- Access: with the `.` notation
- Structs stored in Stack by default to store in Heap use the `&` like `note := &Note{}`
  Heap structs are particularly useful when dealing with structs that carry large amounts of data. Therefore, opting for heap structs can reduce explicit memory allocations.
- struct fields are immutable by default To update a field of a struct, it is necessary that both the field and the variable to which the struct is initialized are mutable
- Access control for struct fields
  | Access modifier | within module | outside module |
  |---|---|---|
  | pub | public | public |
  | mut | mutable | immutable |
  | pub mut | public, mutable | public, immutable |
  | \_global | public, mutable | public, mutable |
- Special fields

  - required
  - default values

  ```v
  import time
  pub struct Note {
  pub:
    id      int
    created time.Time = time.now()
  pub mut:
    message string    [required]
    status  bool
    due     time.Time = time.now().add_days(1)
  }
  ```

- Struct contains another struct (AKA: Inheritance)

  All you need to do is define a struct and ad it as the first field in the child struct

  ```v

  import time

  struct DateInfo {
  created time.Time
  }

  struct Note {
  DateInfo

      id int

      mut:
        content string

  }

  note := Note{
  created: time.now()
  id: 01
  content: 'hello'
  }
  ```

- Defining methods for a struct
  - Receiver
    V allows us to define methods for a struct. A method is a function with a special receiver argument that appears between fn and the method name
    ```v
    fn (r RECEIVER_TYPE) METHOD_NAME(OPTIONAL_INPUT_ARGUMENTS) RETURN_TYPE {
      METHOD BODY
    }
    ```
  - Argument and return pointer
    ```v
    fn postpone(n Note) &Note {
      return &Note {
        NoteTimeInfo: NoteTimeInfo{
          due: n.due.add_days(1)
        }
        id: n.id
        message: n.message
      }
    }
    ```

## Chapter 9: modules

Modular programming pattern help you to organize and separate related functionalities of your code. and module is just a fancy way to say file or dir of files combined to each others. You can start by define `module mod` and import `import mod` then access public members by `module.member` notation

- Rules to deal with modules:

  - create new project `v new proj`
  - to run a project with all the modules inside use `v run .`
  - the main module has file name of the project, placed in the root dir and began with `module main`
  - create new module by creating file `mod1/file1.v` starts with `module mod1` then import on the main module by just `import mod1`
  - Imported modules must be consumed in the code
  - By default, V assumes that a module will be the main module for the files that do not have a module definition
  - Multiple V files in the module (same dir) must define the same module
  - The default scope for members of a module is private so you can only access the public members in the different modules but Both private and public members of a module are accessible from anywhere within the module
  - Cyclic imports are not allowed for a god reason (m2 imports m1 which also import m2 that import m1 which...)
  - Define init functions to execute one-time module-level initializer functionality. is automatically executed when you import the module where it is defined (like a constructor for the class)
    - The init function must only be defined once inside the module.
    - not be marked as public.
    - not accept any input arguments.
    - not have a return type.

- Module types

  - defined: discussed
  - built-in: `import time`
  - package: just a module publish by other coder. will follow the same usage as a module.
    - `v install [module]`: for modules in the public library of V and `v install --git [url]`: for github repos.

## Chapter 10: Concurrency

- Concurrency is a concept, channels are a design pattern
- concurrency and parallelism are two different concepts. but both aims to run tasks simultaneit. However, concurrency focuses on multiple independent tasks to be finished in the least possible time. In contrast to concurrency, parallelism focuses on splitting one single task into multiple resources to speed up finishing a particular job.
- Terminology
  - Program: A program is a set of instructions in the form of functions and statements that help us achieve a particular job.
  - Process: A program when it starts running, is associated with the process. A process can have one or more sub-processes, each of them running on a different thread.
  - Thread: A thread allows one or more tasks to run in sequential order.
  - Task: A task is a unit of work that runs on a thread. It can be represented as a function in V.
- time module

  ```v
  import time
  // current
  time.now()
  // stopwatch
  sw := time.new_stopwatch()
  // sleep
  time.sleep(1 * time.second)

  sw.elapsed().seconds()
  ```

- thread type

  - to declare array of threads all must have the same return type `mut t1 := []thread OPTIONAL_TYPE{}` optional cause it may be void
  - to access the values returned in the thread use `res := t1.wait()`
  - about wait function
    - This function blocks the execution of the main process until the tasks that have been spawned to run concurrently in the other thread have finished executing.
    - This function does not take any input arguments.
    - This function is available on the handle of a single thread, as well as on the variable that was assigned with a thread array filled with handles to concurrent tasks.
    - The return type of the wait() function is similar to that of the function that is being run concurrently.
    - When calling the wait() function on the thread array, all the elements of the thread array must handle the functions with similar return types.

- make a function concurrent just by typing `go` before calling. the return of this calling will be a thread so you can wait on it

  ```v
  res:= go add_task('hi')
  res.wait()

  typeof(res).name // thread
  ```

- Spawning multiple tasks to run concurrently in a list of threads
  ```v
  thread := []thread{}
  thread << go add_task('eat')
  thread << go add_task('drink')
  thread.wait()
  ```
- Comparing sequential and concurrent program runtimes example
- reading values of the thread the return values placed in the `res` as list in the order that we pushed to the threads not on the order of the execution so `thread[0] // output of add_task('eat')`
- Sharing data between the main thread and concurrent tasks

  - only data structures can passed here (general: `struct`, special: `array` or `map`)
  - share in the `shared` access modifier and access with `rlock` to read, `lock` to read/write/modify
  - full example

    ```v
    import rand
    struct Fund {
        name   string
        target f32
    mut:
        total        f32
        num_donors int
    }

    fn (shared f Fund) collect(amt f32) { // the receiver has the shared access modifier
        lock f {                          // read - write lock
            if f.total < f.target {
                f.num_donors += 1
                f.total += amt
                println('$f.num_donors \t before: ${f.total - amt} \t funds received: $amt \t total: $f.total')
            }
        }
    }

    fn donation() f32 {
        return rand.f32_in_range(100.00, 250.00)
    }

    fn main() {
        shared fund := Fund{              // create the variable with shared keyword
            name: 'A noble cause'
            target: 1000.00
        }
        for {
          rlock fund {
            if fund.total >= fund.target {
              break
            }
          }
          h := go donation()
          go fund.collect(h.wait())
        }
        rlock fund { // acquire read lock
            println('$fund.num_donors donors donated for $fund.name')
            println('$fund.name raised total fund amount: \$ $fund.total')
        }
    }
    ```

## Chapter 11: Channels

It is a better way to share values between threads, chan is a queue data structure `FIFO`

- Define

  ```v
  // unbuffered
  uc := chan int{}

  // buferred
  bc := chan int{cap: 2}
  ```

- Arrow operator: `<-` always point to left
- operations
  - push
    ```v
    ch := chan int{cap: 2}
    ch <- 12
    ```
  - pop
    ```v
    output := <- ch
    ```
- properites
  - len
  - cap: the max number of values channel can hold
  - closed
- methods
  - `try_(push/pop)`: these for a debugging needs, and they return a `ChanState` enum has three enum values:
    - not_ready
    - closed
    - success
    ```v
    x := 'hello'
    ch := chan string{cap: 2}
    for {
        status := ch.try_push(x)
        if status == .success {
            println('Channel length: $ch.len')
        } else {
            println('channel status: $status')
            break
        }
    }
    ```
  - `close()`: it is helpful to close the channel after you end the routine
    ```v
    ch := chan int{cap: 2}
    defer {
        ch.close()
    } // Deferred execution to Close channel
    ```
- why unbufferd? the unbuffered channels block the execution when a value is pushed on them. The execution gets blocked because the channel waits until the value gets popped out by another coroutine like `thread.wait()`
  - Example illustrate where that comes handy
    ```v
    fn receiver(ch chan int) {
        println('Received value from the channel ${<-ch}')
    }
    fn main() {
        ch := chan int{}
        defer {
            ch.close()
        }
        t := go receiver(ch) // here you poped before you push
        ch <- 3 // once it pushed it will poped on the other routine
        t.wait() // and here you wait for the other routine
        println('End main')
    }
    ```
- buffered channels are non-blocking channels
- channels select

## Chapter 12: Testing

- Test rules
  - Use the assert keyword to compare the actual and expected outcome. `assert BOOl`
  - A file containing tests must end with the `_test.v` extension.and run with `v hello_test.v` without run
  - Each test is a function and must start with the prefix `test_`. has no arguments and no return
- `testsuit`
  - `testsuit_(begin/end)` useful functions to help prepare and clean the environment before running a test like set and unset an envar
- The AAA pattern of writing tests This pattern provides a clean approach to organizing the instructions that become part of a test case.
  - Arrange: you arrange the inputs needed for the function that you are about to act on. You can also arrange and set expectations such as expected output
  - Act: this could be done by invoking a call to the function under test.
  - Assert: We finally assert whether the results obtained from the act phase are matching the expectations set in the arrange phase.
- Example

  ```v
  import os

  fn testsuite_begin() {
      os.setenv('foo', 'bar', true)
      println('About to start executing all tests')
  }

  fn test_env_foo_has_value_bar() {
      println('Executing test')

      // arrange
      inp := 'foo'
      expected := 'bar'

      // act
      actual := os.getenv(inp)

      // assert
      assert actual == expected
  }

  fn testsuite_end() {
      os.unsetenv('foo')
      println('Finished executing all tests')
  }
  ```

- Test optional return with `or {}`
- Modular testing

  ```
  modulebasics
  |   main.v
  |   main_test.v
  |   v.mod
  |
  \---mod1
            file1.v
            mod1_test.v

  v test .
  ```

- Also you can run `v -stats test mod` to see a detailed output

## Chapter 13: JSON and ORM

JSON: is the most used format to share data in the api

- Decode from a string to be an object (JSON > Struct) it is like parsing a useful data from string so you can deal with it as struct

  ```v

  import json

  struct Note {
    id int
    content string
    completed bool
  }

  fn main() {

    data := '{
      "id": 1,
      "content": "Hello",
      "completed": false
    }'

    extracted_note := json.decode(Note, data) or {
      // if the data has invalid json syntax
      panic("Invalid")
    }

    println(extracted_note)
    println(extracted_note.content) // deal with it as a struct
  }
  ```

- Encoding an object to be data (Struct > JSON) as casting content to be a string useful to send as http response

  ```v
  generated_note := Note {
    id: 2
    content: "bye"
    completed: false
  }
  new_data := json.encode(generated_note)
  println(new_data)
  println(typeof(new_data).name) //string

  ```

ORM

- The ORM module is used to communicate with relational databases (such as Postgres). The communication to the database happens from the programming languages using the objects that are created to reflect the database table schema. (structs)
- There is just one syntax for all SQL dialects, which makes migrating between databases much easier.
- All the queries to interact with the database are written using V's syntax.
- All the queries are automatically sanitized to prevent SQL injection.
- The queries are easier to read and understand.
- The results obtained from a query are automatically converted into objects in V

- Creating a struct with attributes for ORM
  - `[table: "table_name"]`: applied on struct
  - `[sql: col_type; sql: "col_name"]`: applied on field
  - `[primary]` and `[unique; nonull]`
    Example
  ```v
  [table: 'Notes']
  struct Note { // refere to the table with the name of the struct not the table name
      id      int    [primary; sql: serial]
      message string [sql: 'detail'; unique]
      status  bool   [nonull]
  }
  ```
- Connect to the db with builtin sqlite lib

  ```v
  import sqlite

  [table: 'Notes']
  struct Note {
      id      int    [primary; sql: serial]
      message string [sql: 'detail'; unique]
      status  bool   [nonull]
  }

  fn main() {
      db := sqlite.connect('NotesDB.db') or { panic(err) }
  }
  ```

- DDL operations

  - create table
    ```v
    sql db {
        create table Note
    }
    ```
  - drop table
    ```v
    sql db {
        drop table Note
    }
    ```

- DML operations (crud)

  - insert

    ```v
    n1 := Note{
        message: 'Get some milk'
        status: false
    }
    sql db {
        insert n1 into Note
    }
    ```

    you can check `db.last_id()`

  - select: returns an array

  ```v
  notes := sql db {
    select from Note                          // select all
    select from Note order by id desc         // descending
    select from Note order by id desc limit 1
    select from Note where id == 1
  }
  ```

  - update

  ```v
  sql db {
      update Note set status = true where id == 2
  }
  ```

  - delete

  ```v
  sql db {
      delete Note where id == 2
  }
  ```

## Chapter 14: API service

- we will organize our code for the microservice as follows:

  - main.v: This is the logic to set up the web server using vweb.
  - util.v: This is the utility file that holds project-specific constants, structs, and struct methods.
  - note.v: This file will have REST endpoints which perform CRUD operations on the Note table.

- main.v

  ```v
  module main

  import vweb
  import sqlite

  struct App {
    // inherits server proprieties
      vweb.Context

    mut:
      // add db field
      db sqlite.DB
  }

  fn main() {
    // connect to the db
    db := sqlite.connect('notes.db') or { panic(err) }

    // clean old table
    db.exec('drop table if exists Notes')

    // create new instance
    sql db {
      create table Note
    }

    // create app instance and pass the db created
    app := &App {
      db: db
    }

    // run
    vweb.run(app, 8000)
  }
  ```

- util.v

  ```v
  module main

  import json

  struct CustomResponse {
      status  int
      message string
  }

  fn (c CustomResponse) to_json() string {
      return json.encode(c)
  }

  const (
      invalid_json   = 'Invalid JSON Payload'
      note_not_found = 'Note not found'
      unique_message = 'Please provide a unique message for Note'
  )

  ```

## Notes

- Enum vs Struct vs Class
  struct can contain data variables, types and methods. Enum can only contain data types.
  struct supports access specifier. Enum does not.
  Enum is just a collection of symbols like an array.
  but struct and class is almost the same with some differences.
- Struct as receiver vs argument
  The receiver is just a special case of a argument. V/Go provides syntactic sugar to attach methods to types by declaring the first parameter as a receiver.
- Struct is the general form of a data structure in almost low level programming language, but the language also has some special structures like array, map.
- `<<`: shift and append to list (also a list of threads), `<-` push to channel
