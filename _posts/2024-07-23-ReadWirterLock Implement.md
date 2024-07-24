---
layout: single
title:  "ReadWriteLock"
categories: "Network"
Tag: [Network]
toc: true
---
### ReadWriteLock Implementation

This code provides an implementation of a read-write lock. It allows multiple readers or a single writer to access a resource, but not both. The implementation includes spin-locking and yielding to optimize performance. It supports recursive locking for write operations but not for read operations to write operations. The main program demonstrates a simple use case with a writer and a reader task.


```C#
using System;
using System.Threading;
using System.Threading.Tasks;

namespace ServerCore
{
    /// <summary>
    /// 1. No recursive locking allowed.
    /// 2. Spin lock policy (5000 spins -> yield).
    /// 3. Recursive permission YES: WriteLock -> WriteLock, WriteLock -> ReadLock. But ReadLock -> WriteLock is impossible and doesn't make sense.
    /// </summary>
    class Lock // ReadWriterLock Implementation
    {
        const int EMPTY_FLAG = 0x00000000;
        const int WRITE_MASK = 0x7FFF0000;
        const int READ_MASK = 0x0000FFFF;
        const int MAX_SPIN_COUNT = 5000;

        // Using an int for tracking current thread for recursive locking.
        private int _flag;
        private int writeCount = 0; // This doesn't need to be included in the flag.

        public void WriteLock()
        {
            // Check if the same thread already holds the WriteLock.
            int lockThreadId = (_flag & WRITE_MASK) >> 16;
            if (Thread.CurrentThread.ManagedThreadId == lockThreadId)
            {
                writeCount++;
                return;
            }

            // Attempt to acquire the WriteLock.
            while (true)
            {
                int desired = (Thread.CurrentThread.ManagedThreadId << 16) & WRITE_MASK;
                for (int i = 0; i < MAX_SPIN_COUNT; i++)
                {
                    if (Interlocked.CompareExchange(ref _flag, desired, EMPTY_FLAG) == EMPTY_FLAG)
                    {
                        writeCount = 1;
                        return;
                    }
                }

                Thread.Yield();
            }
        }

        public void WriteUnlock()
        {
            int lockCount = --writeCount;
            if (lockCount == 0) Interlocked.Exchange(ref _flag, EMPTY_FLAG);
        }

        public void ReadLock()
        {
            // Check if the same thread already holds the WriteLock.
            int lockThreadId = (_flag & WRITE_MASK) >> 16;
            if (Thread.CurrentThread.ManagedThreadId == lockThreadId)
            {
                Interlocked.Increment(ref _flag);
                return;
            }

            // Attempt to acquire the ReadLock.
            while (true)
            {
                for (int i = 0; i < MAX_SPIN_COUNT; i++)
                {
                    int expected = (_flag & READ_MASK);
                    if (Interlocked.CompareExchange(ref _flag, expected + 1, expected) == expected)
                        return;
                }
                Thread.Yield();
            }
        }

        public void ReadUnlock()
        {
            Interlocked.Decrement(ref _flag);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Example usage
            Lock rwLock = new Lock();
            Task writer = Task.Run(() =>
            {
                rwLock.WriteLock();
                Console.WriteLine("WriteLock acquired");
                Thread.Sleep(1000); // Simulate work
                rwLock.WriteUnlock();
                Console.WriteLine("WriteLock released");
            });

            Task reader = Task.Run(() =>
            {
                rwLock.ReadLock();
                Console.WriteLine("ReadLock acquired");
                Thread.Sleep(500); // Simulate work
                rwLock.ReadUnlock();
                Console.WriteLine("ReadLock released");
            });

            Task.WaitAll(writer, reader);
        }
    }
}

```

