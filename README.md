# Python
This repo contains Advanced topics of python

### Difference between break, continue and pass
    Break - used to exit the loop that is running
    Continue - will not exit the loop but skips all the statements after it
    pass - we declare a function
    Example Code:
        for i in range(1,100):
            if i%3 == 0:
                continue
            print(i)
        print("bye")
    Output if i use break:
        1,2,bye if it hits the target condition it will exit the loop
    Output if i use continue:
        1,2,4,5,7 if it hits the target condition it will skip further execution 3,6,..
    Output if i use pass:
        1,2,3,4,5,6,... if it hits the target condition it i will do nothing
### Shallow copy and deep copy
    A shallow copy creates a new outer object (like a list), but the nested elements inside it still reference the same memory locations as those in the original. So, changes to nested elements affect both the original and the copy.
    In contrast, a deep copy creates a new object and recursively copies all nested elements, ensuring complete independence from the original structure.
    Examples:
        from numpy import *
        # Ways to copy array in python
            a1 =array([1,2,3,4])
            a2 = a1
            print(id(a1), id(a2)) # Here both the arrays are pointing to the array location
            print(a1,a2)
        # Lets do shallow copy
        # Shallow copy means a1,a3 will have different array locations but when we modify a1 values it will be reflected to a3
            a3 = a1.view() # Shallow copy
            print(id(a1), id(a3)) # Will be different locations
            a1[1] = 7
            print(a1, a3) # Will be same values
        # Deep copy since it doesnot have nested values
        # array will be at different locations and changes in the a1 will not be reflected at a2
            a4 = a1.copy()
            a1[1] = 10
            print(a1,a4)
        
        Perfect example
        from copy import deepcopy
        a1 = [[1,2,3],[4,5,6]]
        a2 = a1.copy()
        a1[0] = [7,8,9]
        a1[1][0] = 10 # Modifying nested element
        print(a1, a2)
        a1 = [[1,2,3],[4,5,6]]
        a3 = deepcopy(a1)
        a1[1][0] = 10 # Modifying nested element
        print(a1, a3)
### Ananomous function/Lambda functions
    Functions without names are called ananomous functions
    def square(num):
        return num**2
    print(square(5))
    # Ananomous function
    square = lambda a:a**2
    print(square(5))
### Ternary operator in javascript vs python
    In javascript
        even = 3%2==0 ? True : False
        console(even)
    In python
        even = True if 3%2==0 else False
        print(even)
        In python True condition if condition else elseResult
    Building lambda function for even or odd
        even = lambda x: True if x%2==0 else False
        print(even(10))
    Q) Create a list that contains all the even numbers
    Code:
        a = [1,2,33432,232,45,343,32,232,5534]
        # Fetch all the even numbers for the list
        # Lambda function of even numbers
        even = lambda x: True if x%2==0 else False
        # Using filter
        even_list = list(filter(even, a))
        print(even_list)
        
        # or use this
        even_list = list(filter(lambda x:x%2==0, a))
        print(even_list)
### Map, Filter, Reduce
    Map/Filter/reduce - takes 2 arguments function and list/sequence
    filter - if the target condition is True it returns data, if false it doesnot return
    map - transforms the data
    Code:
        a = [1,2,33432,232,45,343,32,232,5534]
        b = a.copy()
        # Add 2 to every element using Map
            a = list(map(lambda x: x+2, a)) # Here list is important since map returns map sequence
            a = list(map(lambda x: x%2==0, a)) # Here we are transforming data replacing values with true or false this is not best scenario for map
            print(a)
        # Filter
            b = list(filter(lambda x: x%2 ==0, b))
            print(b)
        # Add all the numbers of the array
            from functools import reduce
            a = [1,2,33432,232,45,343,32,232,5534]
            elm_sum = reduce(lambda x,y: x+y, a)
            print(elm_sum)
### Decorators
    Adding a additional functionality to the existing function without modifying the exisisting code
    This decorator functions will contain wrapper function where the existing function is executed and we return the wrapper function
    # Simple decorator code
    Code:
        def my_decorator(func):
            def wrapper():
                print("decorator executed!!")
                func()
            return wrapper
        @my_decorator
        def print_hello():
            print("hello")
        print_hello()

    Q) I had an exisiting function called add_numbers give an additional feature to print average as well to the function
    Code:
        def get_avg(func):
            def wrapper(a,b,c):
                print("avg: ",(a+b+c)/3)
                return func(a,b,c)
            return wrapper
        @get_avg
        def add_nums(a,b,c):
            return a+b+c
        print("sum: ", add_nums(1,2,3))
    Other way of using decorators
    Code:
        def get_avg(func):
            def wrapper(a,b,c):
                print("avg: ",(a+b+c)/3)
                return func(a,b,c)
            return wrapper
        
        def add_nums(a,b,c):
            return a+b+c
        
        #Other way of using decorators are 
        updated_add_nums = get_avg(add_nums)
        print(updated_add_nums(1,2,3))
### Iterators in python
    Iterators are used to iterate over data one item at a time, and they are especially useful for:
    Memory efficiency:
        Iterators don’t store the entire data in memory.
        They generate one item at a time, which is ideal for large datasets or infinite sequences.
    Lazy evaluation:
        Items are computed only when needed, not all at once.
        This makes code more efficient and faster in many cases.
    Maintaining state:
        Iterators remember their position, so each call to next() gives the next item.
        This allows you to pause and resume iteration (e.g., in loops).
    Code:
        # Iterators in python
        a = [1,2,3,4,5,6]
        it = iter(a) # Creates iterator obj
        print(next(it)) # Iterators store one value at a time and it remembers what it had given last time
        print(next(it))
        
        # Iterators in python
        a = [1,2,3,4,5,6]
        it = iter(a) 
        # Print all values
        while(True):
            try:
                print(next(it))
            except StopIteration:
                break
        print("exit")
### Build customized iterator that prints TopTen values
    Code:
        class TopTen:
            def __init__(self):
                self.num = 1
            def __iter__(self):
                return self
            def __next__(self):
                if self.num <= 10:
                    val = self.num
                    self.num+=1
                    return val #Why iam passing val instead of sel.num we i use next function it prints current value not the incremented value
                else:
                    raise StopIteration()
        
        obj = TopTen()
        
        while True:
            try:
                print(next(obj))
            except StopIteration:
                break
        print("exit")
### Why use iterators instead of simple for loop
    a = [1,2,3,4]
    for i in a:
        print(a)
    Although for loops in Python use iterators under the hood, using iterators explicitly offers several advantages:
    Greater control over iteration: You can manually control when to fetch the next item, pause, skip, or resume — which is not possible with a simple for loop.
    Lazy evaluation and memory efficiency: Iterators generate items one at a time, making them ideal for working with large datasets or infinite sequences without consuming much memory.
    Custom iteration logic: You can define your own iterator classes using __iter__() and __next__() to implement complex or non-standard iteration behavior.
    Manual control flow: You can stop and resume iteration exactly where you left off — useful for stateful or interactive programs.
### Generators
    Generators are a simple way to create iterators in Python.
    Unlike regular functions that use return, a generator uses yield to produce a value and pause the function’s state, so it can resume from where it left off.
    Generators automatically return iterator objects — you don't need to call iter() on them.
    Code:
        # Generator function: returns an iterator without using iter()
        def get_nums():
            yield 1  # 'yield' pauses the function and saves its state
            yield 2
        values = get_nums()  # This returns a generator object (an iterator)
        print(next(values))      # Output: 1
        print(next(values))      # Output: 2

        # Retrive top ten square
        def get_nums():
            i = 1
            while i<10:
                sq = i*i
                yield sq
                i+=1
        values = get_nums()
        for i in values:
            print(i)
## Asyncio
    Asyncio is the ideal choice when tasks that wait a lot like network request or reading, It excels in handling many tasks concurrently without using much cpu power, this makes our application more efficient and responsive when your waiting on lot of different tasks.
    Threads are suited for tasks that may need to wait but also share data, they can run in parallel within the same application making them useful for the tasks that are IO bound but less cpu intensive.
    For CPU heavy tasks processors are the way to go. Each process handle individually maximizing cpu usage by running in parallel across multiple courts, this is ideal for intensive computation.
    Asyncio vs threads vs muliprocessing
    Asyncio - For managing many tasks that are waiting
    Threads - For parallel tasks that share data with minimal cpu usage
    Processes - For maximizing performance on CPU intensive tasks
#### Asyncio Event loop and code
    Event Loop - it is the core for managing and distributing the tasks
                 when ever the task gets into the event loop it executed, when the task awaits, it will step aside making enough room to run other task, ensuring the loop is always efficiently utilized, once the awaited operation is complete, the task will resume ensuring smooth and responsive program flow.
    Thats how event loop keep your python program running efficiently handling multiple tasks asyncronously.

    In python if you async for a function it will return coroutine object
    The coroutine object must be awaited to get the result of the exectution.
    If we have 2 coroutines the second coroutine will start running only after the first coroutine is finished
    Example:
        import asyncio
        async def main():
            print("running main!!!")
        print(main()) #coroutine object

    Q) Create a coroutine to fetch data with 2 seconds delay
    Code:
        import asyncio

        # Lets Define a coroutine that simulates time delay task
        async def fetch_data(delay, id):
            print("Fetching data...")
            await asyncio.sleep(delay)
            print("Data Fetched")
            return {"data": f"some data {id}"}
        
        async def main():
            result1 = await fetch_data(2,1)
            print("Results Received", result1)
        
            result2 = await fetch_data(2,2)
            print("Results Received", result2)
            print("End of main")
        
        asyncio.run(main())
    Here the result2 is started running only after result1 Now lets speed up this operation and now run both the coroutines at the same time

#### Tasks
    Task is the way to schedule a coroutine to run as soon as possible and allows us to run multiple coroutines simentanously.
    As soon as the coroutine is sleeping or watiting on something that's not in our control of program, we can move on and start executing other task.
    The issue with the previous is we need wait for one coroutine to be finished for other coroutine needs to be executed.
    we're never going to be executing these tasks at the same time. we're not using multiple cpu cores, but if one task isn't doing something if its idle or if its blocked or if its waiting on something, we can swith over and start woking on another task.
    The whole goal is optimizing its efficiency so we are always attempting to do something and when we are waiting on something that's not in control of our program we switch over to other task and start working on other task 
    Optimizing the previous example
    Code:
        import asyncio

        # Lets Define a coroutine that simulates time delay task
        async def fetch_data(delay, id):
            print("Fetching data...")
            await asyncio.sleep(delay)
            print("Data Fetched")
            return {"data": f"some data {id}"}
        
        async def main():
            task1 = asyncio.create_task(fetch_data(2,1))
            task2 = asyncio.create_task(fetch_data(3,2))
            task3 = asyncio.create_task(fetch_data(1,3))
        
            result1 = await task1
            result2 = await task2
            result3 = await task3
            print(result1, result2, result3)
        
        asyncio.run(main())
#### What if i have task3 that is dependent on task1 and task2. task3 needs to be executed only after task1,2 executed
    Code:
        import asyncio
        
        # Lets Define a coroutine that simulates time delay task
        async def fetch_data(delay, id):
            print("Fetching data...")
            await asyncio.sleep(delay)
            print("Data Fetched")
            return {"data": f"some data {id}"}
        
        async def main():
            task1 = asyncio.create_task(fetch_data(2,1))
            task2 = asyncio.create_task(fetch_data(10,2))
            result1 = await task1
            result2 = await task2
            task3 = asyncio.create_task(fetch_data(1,3))
        
            result3 = await task3
            print(result1, result2, result3)
        
        asyncio.run(main())
### Gather function
    Quick way to run multiple coroutines concurrently. instead of creating create task for every coroutine, we can use gather that will automatically run all the coroutines concurrently and collect the results in the list.
    Its not that great in error handling.
    It is not going to cancel the other coroutines if one of them failed, we get weird state if we are not handling execptions or errors manually.
    Code:
        import asyncio

        # Lets Define a coroutine that simulates time delay task
        async def fetch_data(delay, id):
            print(f"Fetching data{id}...")
            await asyncio.sleep(delay)
            print(f"Data Fetched{id}")
            return {"data": f"some data {id}"}
        
        async def main():
            results = await asyncio.gather(fetch_data(3,1), fetch_data(10,2), fetch_data(2,3))
        
            for result in results:
                print(result)
        
        asyncio.run(main())
    Code output:
        Fetching data1...
        Fetching data2...
        Fetching data3...
        Data Fetched3
        Data Fetched1
        Data Fetched2
        {'data': 'some data 1'}
        {'data': 'some data 2'}
        {'data': 'some data 3'}
    Analysis:
        Here Data3 is fetched first since it takes 1 second to exectute, data1 is fetched next since it takes 2 seconds, data2 is fetched next since it takes 10 seconds
### Task Group
    It has better task management and better error handling
    Part of structured concurrency: tasks are managed within a context block.
    Automatically cancels all other tasks in the group if one fails.
    Easier to read and reason about, especially for complex task trees.
    Better cleanup and lifecycle handling.
    Code:
        import asyncio

        # Lets Define a coroutine that simulates time delay task
        async def fetch_data(delay, id):
            print(f"Fetching data{id}...")
            await asyncio.sleep(delay)
            print(f"Data Fetched{id}")
            return {"data": f"some data {id}"}
        
        async def main():
            tasks = []
            target_task = [2,1,3]
        
            async with asyncio.TaskGroup() as tg:
                for i in range(len(target_task)):
                    task = tg.create_task(fetch_data(target_task[i], i))
                    tasks.append(task)
            results = [task.result() for task in tasks]
            print(results)
        
        asyncio.run(main())
### Difference between gather and TaskGroup
    | Feature                | `asyncio.gather`            | `asyncio.TaskGroup` (3.11+)       |
| ---------------------- | --------------------------- | --------------------------------- |
| Python version         | 3.5+                        | 3.11+                             |
| Structured concurrency | ❌                           | ✅                                 |
| Error handling         | Suppresses extra exceptions | Cancels siblings and raises error |
| Task creation          | Automatic                   | Manual via `create_task()`        |
| Result ordering        | Preserved                   | Manual (you handle result access) |
| Code readability       | Good for small tasks        | Better for complex flows          |

### synchronization (lock)
    lock is to archieve synchronization in coroutines espically when we have larger more complicated programs.
    Lets consider we have a shared resource, it takes some fair amount of time to modify or update on this shared resouce it could be db, or anything.
    and we want to make sure that no two coroutines work on this resouce at the same time
    the reason if two coroutines work at the same time or modifying the same file at same time we might get a error.
    we want to wait for one entire operation to get finished before starting the next operation
    what does async with lock do?
        it will check whether the shared resource is running by other coroutine, if yes then it will wait until the task finished.
                                                                                 if no it will start executing its code block
                                                                                 before lock is released
    Lock is actually synchronizing our different coroutines so they can't be using the block of code while another coroutine is executing. It is locking of access to this critical resource only one will be accessing at a time.
    Code:
        import asyncio

        # Shared resource
        shared_resource = 0
        
        lock = asyncio.Lock()
        
        async def modify_shared_resource():
            global shared_resource
            async with lock:
                # Critical Resource
                print("Resource before modification", shared_resource)
                shared_resource += 1
                asyncio.sleep(2) # Simulating saving in db or saving changes in file
                print("Resource after modifiying", shared_resource)
                # Critical Resource released
        
        async def main():
            asyncio.gather(*(modify_shared_resource() for _ in range(5)))
        
        asyncio.run(main())
### Semaphore
    Allows multiple coroutines to have access to the same object at the same time
    We can even decide how many we wanted to be
    We kind of trottle our program and we dont overload the program
    It is possible to send a bunch of network request at a time not 1000 at a time
    Code:
        import asyncio

        async def access_resource(semaphore, resource_id):
            async with semaphore:
                print("Accessing resource", resource_id)
                await asyncio.sleep(1)
                print("Releasing resouce", resource_id)
        
        
        async def main():
            semaphore = asyncio.Semaphore(2)
            await asyncio.gather(*(access_resource(semaphore, i) for i in range(5)))
        
        asyncio.run(main())
    Output:
        Accessing resource 0
        Accessing resource 1
        Releasing resouce 0
        Releasing resouce 1
        Accessing resource 2
        Accessing resource 3
        Releasing resouce 2
        Releasing resouce 3
        Accessing resource 4
        Releasing resouce 4
    Analysis:
        Accessing 2 resources at a time
## Dunder/ Magic Methods in python
    Dunder stands for double underscore
    In python every thing is a class even Integer, String every thing is class
    Function is also a class
    def print_hello():
        pass
    print(type(print_hello)) #<class 'function'>
    str1="hello"
    str2="world"
    result = str1 + str2 #helloworld
    How does that work?
        In the string there is method called __add__ that is equivalent to +, __sub__ that is equivalent to -
        now the actual code is
        result = str1.__add__(str2)
        This __add__ method is already defined the string class
        Similarly
        len(str1) -> str1.__len__()
    Lets create a Counter class
    Code:
        # Lets create a counter class that should contain add method
        class Counter:
            def __init__(self):
                self.value = 1
            
            def count_up(self):
                self.value += 1
            
            def count_down(self):
                self.value -= 1
            
            def __add__(self, other):
                if isinstance(other, Counter): #Checking other counter is of same class
                    return self.value + other.value
                raise Exception("Invalid type")
            
            # It is meant for user friendly output
            def __str__(self):
                return f"{self.value}"
            
            # It is meant for More detailed output Developer friendly
            def __repr__(self):
                return f"Counter={self.value}"
        
        counter1 = Counter()
        counter2 = Counter()
        
        counter1.count_up()
        counter2.count_up()
        counter2.count_up()
        
        print(counter1) # Since __str__ is implemented we could be able to see it if not we would see the address of the object
        print(repr(counter1))
        
        print(counter1+counter2)
        
## List Comprehension
    Code:
        # List comprehension
        a = [x for x in range(10)]
        print(a)
        
        # Get all the even numbers from 1 to 50
        even = []
        for i in range(1, 50):
            if(i%2 == 0):
                even.append(i)
        print(even)
        
        # Q) Use List comprehension for the above example
        even = [x for x in range(1,50) if x%2==0]
        print(even)
        
        # Q) Check weather a word starts with a and ends with y
        words = ["abc", "abcy", "abcdy"]
        result = []
        for word in words:
            if len(word) >= 2:
                if word[0] == "a":
                    if word[-1] == "y":
                        result.append(word)
        print(result)
        
        # List comprehension
        # In the list comprehension we can write multiple if statements
        result = [
            word
            for word in words
            if len(word) >= 2
            if word[0] == "a"
            if word[-1] == "y"
        ]
        print(result)
        
        # Q) Flattening the matrix
        matrix = [[1,2],[3,4],[5,6]]
        flattend_list = []
        
        for row in matrix:
            for elm in row:
                flattend_list.append(elm)
        print(flattend_list)
        
        flattend_list = [elm for row in matrix for elm in row]
        print(flattend_list)
        
        # Q) Categorize Even and odd
        category = ["Even" if elm%2==0 else "Odd" for elm in range(10)]
        print(category)
        
        # If i want to use only if statement
        category = ["Even" for elm in range(10) if elm%2==0]
        print(category)
#### List comprehension main examples
    Code:
        # In the list comprehension we can write multiple if statements
        result = [
            word
            for word in words
            if len(word) >= 2
            if word[0] == "a"
            if word[-1] == "y"
        ]
        print(result)
        
        # Q) Categorize Even and odd using if else statement
        category = ["Even" if elm%2==0 else "Odd" for elm in range(10)]
        print(category)
        
        # If i want to use only if statement
        category = ["Even" for elm in range(10) if elm%2==0]
        print(category)

        # Function call
        def square(x):
            return x*x
        square_list = [square(x) for x in range(10)]
        print(square_list)
        
        # Use lambda
        square = lambda x: x*x
        square_list = [square(x) for x in range(10)]
        print(square_list)
### Dictionary comprehension
    a = [("a",1), ("b",2)]
    square = lambda x: x*2
    dict_a = {k:square(v) for k,v in a}
    print(dict_a)

    # Set comprehension
    a = [1,2,3,2,3,4,5,6]
    set_a = {x**2 for x in a}
    print(set_a)


    



    

    
        




    


        
