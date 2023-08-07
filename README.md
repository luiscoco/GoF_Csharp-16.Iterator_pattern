# GoF_Csharp-16.Iterator_pattern


```csharp
using System;
using System.Collections;
using System.Collections.Generic;

public class Program
{
    public static void Main()
    {
        int[] items = { 1, 2, 3, 4, 5 };
        SimpleCollection collection = new SimpleCollection(items);

        // Using the iterator to traverse the collection
        IIterator iterator = collection.GetIterator();
        while (iterator.HasNext())
        {
            int item = iterator.Next();
            Console.WriteLine(item);
        }

        // Alternatively, using foreach with IEnumerable
        foreach (int item in collection)
        {
            Console.WriteLine(item);
        }
    }
}

// Step 1: Iterator Interface
public interface IIterator
{
    bool HasNext();
    int Next();
}

// Step 2: Iterator Class
public class SimpleIterator : IIterator, IEnumerator<int>
{
    private SimpleCollection collection;
    private int currentIndex;

    public SimpleIterator(SimpleCollection collection)
    {
        this.collection = collection;
        this.currentIndex = -1; // Start at -1 to indicate the initial position before the first element
    }

    // IIterator interface methods
    public bool HasNext()
    {
        return currentIndex + 1 < collection.Items.Length;
    }

    public int Next()
    {
        currentIndex++;
        return collection.Items[currentIndex];
    }

    // IEnumerator<int> interface methods
    public int Current => collection.Items[currentIndex];

    object IEnumerator.Current => collection.Items[currentIndex];

    public bool MoveNext()
    {
        currentIndex++;
        return currentIndex < collection.Items.Length;
    }

    public void Reset()
    {
        currentIndex = -1;
    }

    public void Dispose()
    {
        // No resources to dispose in this case
    }
}

// Step 3: Aggregate Class
public class SimpleCollection : IEnumerable<int> // This makes the collection enumerable of integers
{
    public int[] Items { get; }

    public SimpleCollection(int[] items)
    {
        Items = items;
    }

    // Method to get the iterator for the collection
    public IIterator GetIterator()
    {
        return new SimpleIterator(this);
    }

    // IEnumerable implementation for iteration using foreach
    public IEnumerator<int> GetEnumerator()
    {
        return new SimpleIterator(this);
    }

    // Non-generic IEnumerator implementation (required for IEnumerable)
    IEnumerator IEnumerable.GetEnumerator()
    {
        return new SimpleIterator(this);
    }
}
```

## How to setup Github actions

![Csharp Github actions](https://github.com/luiscoco/GoF_Csharp-16.Iterator_pattern/assets/32194879/1263a83b-d11c-4a48-ad5c-c22eecd42836)



