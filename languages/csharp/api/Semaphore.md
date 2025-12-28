# 🚃Semaphore

Semaphores in C# work a bit like what Kafka offers.  
Here, you will define in your semaphore the number of requests it  
can handle at the same time (initialCount) and the maximum number of requests that can be released simultaneously (maximumCount). The latter must be greater than or equal to the number of requests it can handle initially, otherwise an exception will be raised.

## 👨🏻‍💻Implementation

### 🤏🏻SemaphoreSlim

Allows to create a semaphore for processes in C# and therefore does not interact directly with the operating system. Which is recommended and much more optimized than the native semaphore.

```C#
private static Semaphore semaphore = new Semaphore(initialCount: 3, maximumCount: 3);

    static void Main()
    {        
	    for (var i = 0; i < 10; i++)
        {
            var threadNum = i;
            Thread t = new Thread(() =>
            {
                Console.WriteLine($"Thread {threadNum} is waiting to enter...");
                semaphore.WaitOne();  // Acquire the semaphore
                Console.WriteLine($"Thread {threadNum} has entered.");
                
                Thread.Sleep(1000);   // Simulate some work
                
                Console.WriteLine($"Thread {threadNum} is leaving.");
                semaphore.Release();  // Release the semaphore
            });
            t.Start();
        }
    }
```

### 🚦Semaphore

Here, we use system semaphores. It is possible to make calls between several programs because they are named.

```C#
using System;
using System.Threading;

class Program
{
	// Creation of a named (or unnamed) semaphore with an initial counter of 3, maximum 3  
	// The named semaphore allows sharing between multiple processes
    static Semaphore semaphore = new Semaphore(initialCount: 3, maximumCount: 3, name: "GlobalSemaphoreExample");

    static void Main()
    {
        // Let's create 5 threads simulating processes attempting to access the resource
        for (int i = 1; i <= 5; i++)
        {
            int threadId = i;
            new Thread(() =>
            {
                Console.WriteLine($"Thread {threadId} attend pour entrer...");
                semaphore.WaitOne();  // Demande d'entrée (bloque si compteur à 0)
                try
                {
                    Console.WriteLine($"Thread {threadId} est dans la section critique.");
                    Thread.Sleep(2000);  // Simulate a work
                    Console.WriteLine($"Thread {threadId} quitte la section critique.");
                }
                finally
                {
                    semaphore.Release();  // Release of the resource
                }
            }).Start();
        }
        Console.ReadLine();
    }
}
```


