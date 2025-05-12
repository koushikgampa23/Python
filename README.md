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


    
        




    


        
