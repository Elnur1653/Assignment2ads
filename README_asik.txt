This code is an implementation of a custom dynamic array in Java named MyArrayList. Let’s break it down beautifully and thoroughly, and in a way that's as human-like as possible:

Purpose of the class: The MyArrayList class builds a data structure similar to Java's standard ArrayList, but with its own logic. It allows storage, dynamic resizing, addition, removal, and retrieval of elements, alongside other handy operations.

My Array List

Key Components of the Code
1. Fields:
- data: This array serves as the container for the elements.
- size: Tracks the number of elements currently stored.
- INITIAL_CAPACITY: Defines the starting size of the array, set to 10.

2. Constructor:
- The MyArrayList() method initializes the array with the default capacity (INITIAL_CAPACITY) and sets the size to 0.


3. Expanding the Array:
- The ensureCapacity() method checks if there’s enough space in the array. If not, it doubles its size and copies the old elements into the new, larger array.


4. Adding Elements:
- add(T item): Appends a new item to the end of the list.
- add(int index, T item): Inserts a new item at the specified index, shifting existing elements to the right.
- addFirst(T item) and addLast(T item): Specialized methods for adding elements at the beginning and end of the list.


5. Accessing and Modifying Elements:
- get(int index): Fetches the element at the given index.
- getFirst() and getLast(): Retrieve the first and last elements of the list.
- set(int index, T item): Updates the element at the specified index.


6. Removing Elements:
- remove(int index): Deletes the element at the given index and shifts remaining elements to fill the gap.
- removeFirst() and removeLast(): Removes the first or last elements of the list.


7. Additional Utilities:
- indexOf(Object object) and lastIndexOf(Object object): Locate the first and last occurrences of a given object within the list.
- exists(Object object): Checks whether a specific object exists in the list.
- toArray(): Converts the contents of the list into a standard array.
- clear(): Clears all elements from the list, resetting its size.


8. Iterating Over the List:
- An inner class provides functionality for iterating through the list using an iterator.


9. Unimplemented Feature:
- The sort() method currently throws an exception, indicating that sorting functionality has yet to be implemented.


Why this code is elegant: This class illustrates fundamental concepts of data structures:
- Dynamic resizing ensures efficient memory management.
- Index validation guarantees safe access to elements.
- Flexible manipulation of elements: adding, removing, searching, and iterating through items.

public class MyArrayList<T> implements MyList<T> {
    private Object[] data;
    private int size;
    private static final int INITIAL_CAPACITY = 10;
    public MyArrayList() {
        data = new Object[INITIAL_CAPACITY];
        size = 0;
    }
    private void ensureCapacity() {
        if (size >= data.length) {
            Object[] newData = new Object[data.length * 2];
            System.arraycopy(data, 0, newData, 0, data.length);
            data = newData;
        }
    }
    @Override
    public void add(T item) {
        ensureCapacity();
        data[size++] = item;
    }

    public void set(int index, T item) {
        checkIndex(index);
        data[index] = item;
    }

    public void add(int index, T item) {
        checkIndexForAdd(index);
        ensureCapacity();
        System.arraycopy(data, index, data, index + 1, size - index);
        data[index] = item;
        size++;
    }
    public void addFirst(T item) {
        add(0, item);
    }
    public void addLast(T item) {
        add(item);
    }
    public T get(int index) {
        checkIndex(index);
        return (T) data[index];
    }
    public T getFirst() {
        return get(0);
    }
    public T getLast() {
        return get(size - 1);
    }
    public void remove(int index) {
        checkIndex(index);
        System.arraycopy(data, index + 1, data, index, size - index - 1);
        data[--size] = null;
    }
    public void removeFirst() {
        remove(0);
    }
    public void removeLast() {
        remove(size - 1);
    }
    public void sort() {
        // Optionally implement later depending on element type
        throw new UnsupportedOperationException("Sort not implemented.");
    }
    public int indexOf(Object object) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(object)) return i;
        }
        return -1;
    }
    public int lastIndexOf(Object object) {
        for (int i = size - 1; i >= 0; i--) {
            if (data[i].equals(object)) return i;
        }
        return -1;
    }

    @Override
    public boolean exists(Object object) {
        return indexOf(object) != -1;
    }

    @Override
    public Object[] toArray() {
        Object[] result = new Object[size];
        System.arraycopy(data, 0, result, 0, size);
        return result;
    }

    @Override
    public void clear() {
        for (int i = 0; i < size; i++) data[i] = null;
        size = 0;
    }

    @Override
    public int size() {
        return size;
    }

    private void checkIndex(int index) {
        if (index < 0 || index >= size) throw new IndexOutOfBoundsException();
    }

    private void checkIndexForAdd(int index) {
        if (index < 0 || index > size) throw new IndexOutOfBoundsException();
    }

    @Override
    public java.util.Iterator<T> iterator() {
        return new java.util.Iterator<T>() {
            private int current = 0;

            @Override
            public boolean hasNext() {
                return current < size;
            }

            @Override
            public T next() {
                return (T) data[current++];
            }
        };
    }
}







My Linked List



Key Components of the Code
1. Inner Class - Node:
- Node is a private inner class representing an element in the linked list.
- Each Node contains:- value: The actual data stored.
- next: A reference to the next node.
- prev: A reference to the previous node.



2. Fields:
- head: The first node in the list.
- tail: The last node in the list.
- size: The current number of elements in the list.



3. Constructor:
- MyLinkedList(): Initializes an empty linked list, setting head and tail to null and size to 0.


4. Adding Elements:
- add(T item): Adds an element to the end of the list.
- addFirst(T item): Adds an element to the beginning of the list.
- addLast(T item): Adds an element to the end of the list (used by add).
- add(int index, T item): Inserts an element at a specific index.- If the index is 0, it calls addFirst().
- If the index equals size, it calls addLast().
- Otherwise, it inserts the element between two existing nodes.



5. Retrieving Elements:
- get(int index): Returns the value at the specified index.
- getFirst(): Returns the value of the first node.
- getLast(): Returns the value of the last node.



6. Removing Elements:
- remove(int index): Removes the node at the specified index.
- removeFirst(): Removes the first node.
- removeLast(): Removes the last node.

Helper Method:
- unlink(Node node): Removes a specific node by updating the references of its prev and next neighbors.


7. Accessing Nodes:
- getNode(int index): Retrieves the node at the given index. It optimizes traversal:- If the index is in the first half of the list, it starts from head.
- If the index is in the second half, it starts from tail.



8. Searching:
- indexOf(Object object): Finds the index of the first occurrence of an element.
- lastIndexOf(Object object): Finds the index of the last occurrence of an element.
- exists(Object object): Checks if an element exists in the list.


9. Other Utilities:
- toArray(): Converts the linked list into an array.
- clear(): Empties the linked list by setting head and tail to null and resetting the size to 0.
- size(): Returns the current size of the list.


10. Iterator:
- Provides an iterator to traverse the list using hasNext() and next().


11. Unimplemented Feature:
- The sort() method is a placeholder, currently throwing an exception. Sorting is yet to be implemented.


public class MyLinkedList<T> implements MyList<T> {
    private class Node {
        T value;
        Node next;
        Node prev;
        Node(T value) {
            this.value = value;
        }
    }
    private Node head;
    private Node tail;
    private int size;
    public MyLinkedList() {
        head = null;
        tail = null;
        size = 0;
    }
    @Override
    public void add(T item) {
        addLast(item);
    }
    public void set(int index, T item) {
        Node node = getNode(index);
        node.value = item;
    }
    public void add(int index, T item) {
        if (index < 0 || index > size) throw new IndexOutOfBoundsException();
        if (index == 0) {
            addFirst(item);
        } else if (index == size) {
            addLast(item);
        } else {
            Node current = getNode(index);
            Node newNode = new Node(item);
            Node prevNode = current.prev;
            newNode.next = current;
            newNode.prev = prevNode;
            prevNode.next = newNode;
            current.prev = newNode;
            size++;
        }
    }
    public void addFirst(T item) {
        Node newNode = new Node(item);
        if (head == null) {
            head = tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
    }
    public void addLast(T item) {
        Node newNode = new Node(item);
        if (tail == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }
        size++;
    }
    public T get(int index) {
        return getNode(index).value;
    }
    public T getFirst() {
        if (head == null) throw new IllegalStateException("List is empty");
        return head.value;
    }
    public T getLast() {
        if (tail == null) throw new IllegalStateException("List is empty");
        return tail.value;
    }
    public void remove(int index) {
        Node node = getNode(index);
        unlink(node);
    }
    public void removeFirst() {
        if (head == null) throw new IllegalStateException("List is empty");
        unlink(head);
    }
    public void removeLast() {
        if (tail == null) throw new IllegalStateException("List is empty");
        unlink(tail);
    }

    private void unlink(Node node) {
        Node prev = node.prev;
        Node next = node.next;

        if (prev == null) head = next;
        else prev.next = next;

        if (next == null) tail = prev;
        else next.prev = prev;

        size--;
    }

    private Node getNode(int index) {
        if (index < 0 || index >= size) throw new IndexOutOfBoundsException();

        Node current;
        if (index < size / 2) {
            current = head;
            for (int i = 0; i < index; i++) current = current.next;
        } else {
            current = tail;
            for (int i = size - 1; i > index; i--) current = current.prev;
        }
        return current;
    }
    public void sort() {
        throw new UnsupportedOperationException("Sort not implemented.");
    }
    public int indexOf(Object object) {
        Node current = head;
        int index = 0;
        while (current != null) {
            if (current.value.equals(object)) return index;
            current = current.next;
            index++;
        }
        return -1;
    }

    @Override
    public int lastIndexOf(Object object) {
        Node current = tail;
        int index = size - 1;
        while (current != null) {
            if (current.value.equals(object)) return index;
            current = current.prev;
            index--;
        }
        return -1;
    }
    public boolean exists(Object object) {
        return indexOf(object) != -1;
    }
    public Object[] toArray() {
        Object[] array = new Object[size];
        Node current = head;
        for (int i = 0; i < size; i++) {
            array[i] = current.value;
            current = current.next;
        }
        return array;
    }
    public void clear() {
        head = null;
        tail = null;
        size = 0;
    }
    public int size() {
        return size;
    }
    public java.util.Iterator<T> iterator() {
        return new java.util.Iterator<T>() {
            private Node current = head;
            public boolean hasNext() {
                return current != null;
            }
            public T next() {
                T value = current.value;
                current = current.next;
                return value;
            }
        };
    }
}










Stack



1. Fields and constructor

private MyList<T> list;

public MyStack() {
    list = new MyArrayList<>();
}


You create a variable called list that holds all the stack's elements.
It uses your custom MyArrayList class.
So basically, your stack is built on top of your own array list.

2. Push operation

public void push(T item) {
    list.addLast(item); // Push to the end
}


This adds a new item to the top of the stack.
It just adds it to the end of the list (which is the top of the stack).
Like putting a new plate on top of a stack.

3. Pop operation

public T pop() {
    if (isEmpty()) throw new IllegalStateException("Stack is empty");
    T top = list.getLast();
    list.removeLast();
    return top;
}

This removes and returns the top item of the stack.
First, it checks if the stack is empty.
Then it grabs the last element in the list (top of stack), removes it, and returns it.
Like removing the top plate from the stack.


4. Peek

public T peek() {
    if (isEmpty()) throw new IllegalStateException("Stack is empty");
    return list.getLast();
}

This just shows you the top item without removing it.
Like looking at the top plate without touching the others.

5. isEmpty() and size()

public boolean isEmpty() {
    return list.size() == 0;
}

public int size() {
    return list.size();
}
isEmpty() checks if there are any elements in the stack.

size() tells how many items are currently in the stack.

6. Clear the stack

public void clear() {
    list.clear();
}
This deletes all elements from the stack.

7. toString() method

@Override
public String toString() {
    Object[] items = list.toArray();
    StringBuilder sb = new StringBuilder("Top -> ");
    for (int i = items.length - 1; i >= 0; i--) {
        sb.append(items[i]).append(" ");
    }
    return sb.toString();
}
This method is just for printing the stack in a readable way.

It shows elements from top to bottom, like this:


Top -> 9 7 5 3

✅ Summary in 1 sentence:
This class builds a stack using your own array list (MyArrayList) and supports adding, removing, viewing, clearing, and printing items — just like any real stack should behave.


public class MyStack<T> {
    private MyList<T> list;
    public MyStack() {
        list = new MyArrayList<>();
    }
    public void push(T item) {
        list.addLast(item); // Push to the end
    }
    public T pop() {
        if (isEmpty()) throw new IllegalStateException("Stack is empty");
        T top = list.getLast();
        list.removeLast();
        return top;
    }
    public T peek() {
        if (isEmpty()) throw new IllegalStateException("Stack is empty");
        return list.getLast();
    }
    public boolean isEmpty() {
        return list.size() == 0;
    }
    public int size() {
        return list.size();
    }
    public void clear() {
        list.clear();
    }
    @Override
    public String toString() {
        Object[] items = list.toArray();
        StringBuilder sb = new StringBuilder("Top -> ");
        for (int i = items.length - 1; i >= 0; i--) {
            sb.append(items[i]).append(" ");
        }
        return sb.toString();
    }
}











Queue


1. It stores data in a list
The queue uses your own MyLinkedList (or you could use MyArrayList) to hold the data.
So, every item in the queue is actually stored in a linked list that you built earlier.

2. Adding items (enqueue)
When you want to add something to the queue, you place it at the end.
This is like someone joining the end of the line.

3. Removing items (dequeue)
When you remove something from the queue, you take it from the front — the item that’s been waiting the longest.
Just like letting the first person in line go first.

4. Peeking
This means just looking at the first item in the queue without removing it.
It lets you check who’s next without actually taking them out of the line.

5. Checking if the queue is empty
There’s a method that tells you whether the queue has any items in it.
It returns true if the queue is empty, false otherwise.

6. Getting the size
You can also check how many items are currently in the queue.

7. Clearing the queue
There’s a function that completely clears the queue — as if everyone left and the line is empty again.

8. String version (toString)
This just helps you print the whole queue in a readable way.
It shows the items from front to back, like this:

Front -> A B C <- Rear
So you can see who’s in the line and in what order.

✅ In simple words:
This class behaves exactly like a real-life queue:

You add people at the back

You let people out from the front

You can check who’s next, how many are in line, or clear the whole thing.


public class MyQueue<T> {
    private MyList<T> list;
    public MyQueue() {
        list = new MyLinkedList<>();
    }
    public void enqueue(T item) {
        list.addLast(item); // Add to the end of the queue
    }
    public T dequeue() {
        if (isEmpty()) throw new IllegalStateException("Queue is empty");
        T front = list.getFirst();
        list.removeFirst(); // Remove from the front
        return front;
    }
    public T peek() {
        if (isEmpty()) throw new IllegalStateException("Queue is empty");
        return list.getFirst();
    }
    public boolean isEmpty() {
        return list.size() == 0;
    }
    public int size() {
        return list.size();
    }
    public void clear() {
        list.clear();
    }
    @Override
    public String toString() {
        Object[] items = list.toArray();
        StringBuilder sb = new StringBuilder("Front -> ");
        for (Object item : items) {
            sb.append(item).append(" ");
        }
        sb.append("<- Rear");
        return sb.toString();
    }
}










Minimum Heap



1. Storage
You're using your own MyArrayList to store elements. Think of it like a dynamic array that behaves like a binary tree.

2. Adding elements (insert)
When you insert something:

It goes to the end of the array (like the next empty spot in a tree)

Then it “bubbles up” (using heapifyUp) to make sure the smallest element is still at the top

It's like you drop a new box into a pyramid, and if it's lighter, it floats up until it's in the right spot.

3. Getting the smallest (extractMin)
When you take out the minimum:

You grab the first item (top of the heap)

Move the last item to the top

Then you “push it down” (using heapifyDown) to restore the heap structure

It’s like removing the lightest box from the top, and reorganizing the rest so the next lightest comes up.

4. Peeking
You can look at the smallest item without removing it — just to see what’s at the top.

5. Checking size and clearing
You can check how many items are in the heap or clear everything and start fresh.

6. Keeping the order right
heapifyUp is called when adding: it moves items up to maintain the heap rule.

heapifyDown is called when removing: it moves the new root down until it finds the right place.

7. Swapping elements
There’s a helper method that just swaps two items in the array. It’s used during reordering.

8. Printing the heap
The toString() method turns the whole heap into a readable line of text so you can see all the elements easily.

✅ Summary
This class builds a Min Heap using your own dynamic list. It always keeps the smallest item on top, and supports inserting, removing, checking the top, and clearing the heap — all while keeping the internal order correct using “bubble up” and “push down” techniques.



import java.util.Comparator;
public class MyMinHeap<T extends Comparable<T>> {
    private MyList<T> heap;
    public MyMinHeap() {
        heap = new MyArrayList<>();
    }
    public void insert(T item) {
        heap.addLast(item);
        heapifyUp(heap.size() - 1);
    }
    public T extractMin() {
        if (isEmpty()) throw new IllegalStateException("Heap is empty");
        T min = heap.get(0);
        T last = heap.get(heap.size() - 1);
        heap.set(0, last);
        heap.removeLast();
        heapifyDown(0);
        return min;
    }
    public T peek() {
        if (isEmpty()) throw new IllegalStateException("Heap is empty");
        return heap.get(0);
    }
    public boolean isEmpty() {
        return heap.size() == 0;
    }
    public int size() {
        return heap.size();
    }
    public void clear() {
        heap.clear();
    }
    private void heapifyUp(int index) {
        while (index > 0) {
            int parentIdx = (index - 1) / 2;
            if (heap.get(index).compareTo(heap.get(parentIdx)) < 0) {
                swap(index, parentIdx);
                index = parentIdx;
            } else {
                break;
            }
        }
    }
    private void heapifyDown(int index) {
        int size = heap.size();
        while (index < size) {
            int left = 2 * index + 1;
            int right = 2 * index + 2;
            int smallest = index;

            if (left < size && heap.get(left).compareTo(heap.get(smallest)) < 0) {
                smallest = left;
            }
            if (right < size && heap.get(right).compareTo(heap.get(smallest)) < 0) {
                smallest = right;
            }
            if (smallest != index) {
                swap(index, smallest);
                index = smallest;
            } else {
                break;
            }
        }
    }
    private void swap(int i, int j) {
        T temp = heap.get(i);
        heap.set(i, heap.get(j));
        heap.set(j, temp);
    }
    @Override
    public String toString() {
        Object[] items = heap.toArray();
        StringBuilder sb = new StringBuilder("Heap: ");
        for (Object item : items) {
            sb.append(item).append(" ");
        }
        return sb.toString();
    }
}

