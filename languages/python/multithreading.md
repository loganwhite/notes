# Python multithreading and parallel processing
from https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/#what-should-you-use-
# multithreading 
Python is a linear language, but the threading module comes in handy when you want a little more processing power. While threading in Python cannot be used for parallel CPU computation, it's perfect for I/O operations such as web scraping because the processor is sitting idle waiting for data.

Threading is game-changing because many scripts related to network/data I/O spend the majority of their time waiting for data from a remote source. Because downloads might not be linked (i.e., scraping separate websites), the processor can download from different data sources in parallel and combine the result at the end. For CPU intensive processes, there is **little benefit** to using the threading module, or **even worse**.

**In a word, threading share the same data space and when multiple threading is enabled, only one thread can be executed at a time, and the multicore feature cannot be used**

Without multiprocessing, Python programs have trouble maxing out your system's specs because of the GIL (Global Interpreter Lock). Python wasn't designed considering that personal computers might have more than one core (shows you how old the language is), so the GIL is necessary because Python is not thread-safe and there is a globally enforced lock when accessing a Python object. Though not perfect, it's a pretty effective mechanism for memory management. What can we do?


# multiprocessing
Multiprocessing allows you to create programs that can run concurrently (bypassing the GIL) and use the entirety of your CPU core. Though it is fundamentally different from the threading library, the syntax is quite similar. The multiprocessing library gives each process its own Python interpreter and each their own GIL.

Because of this, the usual problems associated with threading (such as data corruption and deadlocks) are no longer an issue. Since the processes don't share memory, they can't modify the same memory concurrently.

Let's Get Started:
```python3
import multiprocessing
def spawn():
  print('test!')

if __name__ == '__main__':
  for i in range(5):
    p = multiprocessing.Process(target=spawn)
    p.start()
```
If you have a shared database, you want to make sure that you're waiting for relevant processes to finish before starting new ones.

```python3
for i in range(5):
  p = multiprocessing.Process(target=spawn)
  p.start()
  p.join() # this line allows you to wait for processes
```
If you want to pass arguments to your process, you can do that with args

```python3
import multiprocessing
def spawn(num):
  print(num)

if __name__ == '__main__':
  for i in range(25):
    ## right here
    p = multiprocessing.Process(target=spawn, args=(i,))
    p.start()
```
Here's a neat example because as you notice, the numbers don't come in the order you'd expect (without the p.join()).

As with threading, there are still drawbacks with multiprocessing ... you've got to pick your poison:

- There is I/O overhead from data being shuffled around between processes

The entire memory is copied into each subprocess, which can be a lot of overhead for more significant programs
What Should You Use?

If your code has a lot of I/O or Network usage:

- Multithreading is your best bet because of its low overhead

If you have a GUI

- Multithreading so your UI thread doesn't get locked up

If your code is CPU bound:

- You should use multiprocessing (if your machine has multiple cores)
