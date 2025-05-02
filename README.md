using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ADS103_Banki_Balazs_A00134575_A3_Task2
{
    internal class Program
    {
        static void Main(string[] args)
        {
            CEO ceo1 = new CEO("John Doe", 100000, 20000);                          // instantiate objects of CEO, Programmer, and Janitor classes
            Programmer programmer1 = new Programmer("Jane Smith", 80000, 5, 2);
            Janitor janitor1 = new Janitor("Bob Johnson", 40000);
            CEO ceo2 = new CEO("Alice Brown", 120000, 25000);
            Programmer programmer2 = new Programmer("Charlie Davis", 90000, 10, 3);
            Janitor janitor2 = new Janitor("Eve Wilson", 45000);
            CEO ceo3 = new CEO("David Miller", 110000, 22000);
            Programmer programmer3 = new Programmer("Grace Lee", 85000, 7, 1);
            Janitor janitor3 = new Janitor("Hank Taylor", 42000);
            CEO ceo4 = new CEO("Ivy Anderson", 130000, 30000);
            MaxHeap maxheap = new MaxHeap();                                        // instantiate the object of the MaxHeap class
            maxheap.Insert(ceo1);                                                   // inserting objects into the heap
            maxheap.Insert(programmer1);
            maxheap.Insert(janitor1);
            maxheap.Insert(ceo2);
            maxheap.Insert(programmer2);
            maxheap.Insert(janitor2);
            maxheap.Insert(ceo3);
            maxheap.Insert(programmer3);
            maxheap.Insert(janitor3);
            maxheap.Insert(ceo4);
            maxheap.PrintHeap();                                                    // calling the PrintHeap method to display the heap
            Console.WriteLine("\nExtracting max elements with while loop:\n");  // displaying message
            while ( maxheap.heap != null && maxheap.heap.Length > 0)            // while loop to extract max elements
            {
                Employee max = maxheap.ExtractMax(); // creating max object of the employee class & assigning to extracting max element
                Console.WriteLine($"Extracted Max: ");  // displaying message
                max.OutputEarnings();                   // calling the OutputEarnings method to display the max element
                max.OutputJobDescription();             // calling the OutputJobDescription method to display the max element
                Console.WriteLine();                // printing off an empty line
            }
            maxheap.PrintHeap();            // calling the PrintHeap method to display the heap
        }
        public class Employee                               // creating employee class
        {
            public string Name { get; set; }                // employee class properties
            public int Salary { get; set; }
            public Employee( string name, int salary)       // employee class constructor with properties
            {
                this.Name = name;                                   // class constructor properties
                this.Salary = salary;
            }
            virtual public void OutputEarnings()                        // method of the class 
            {
                Console.WriteLine($"Name: {Name}, Salary: {Salary}");       // action of the method
            }
            virtual public void OutputJobDescription()                      // method of the class
            {
                Console.WriteLine("Job Description: " + GetType().Name);    // action of the method
            }
        }
        public class CEO : Employee                                             // child inherited class of employee 
        {
            public int Bonus { get; set; }                                      // properties of child class
            public CEO(string name, int salary, int bonus) : base(name, salary) // child class constructor with properties
            {
                this.Bonus = bonus;
            }
            public override void OutputEarnings()                                       // overriden method of child class
            {
                Console.WriteLine($"Name: {Name}, Salary: {Salary}, Bonus:{Bonus}");        // action of the method
            }
            public override void OutputJobDescription()                                 // overriden method of child class
            {
                Console.WriteLine("Job Description: " + GetType().Name);                    // action of the method
            }
        }
        public class Programmer : Employee                                          // child inherited class of employee
        {
            public int bugsFixed { get; set; }                                                       // properties of child class
            public int bugsCreated { get; set; }                                                    // properties of child class
            public Programmer(string name, int salary, int bugsFixed, int bugsCreated) : base(name, salary)
            {                                                                               // child class constructor with properties
                this.bugsFixed = bugsFixed;
                this.bugsCreated = bugsCreated;
            }
            public override void OutputEarnings()                           // overriden method of child class
            {
                Console.WriteLine($"Name: {Name}, Salary: {Salary}");       // action of the method
            }
            public override void OutputJobDescription()                         // overriden method of child class
            {                                                                   // action of the method
                Console.WriteLine($"Job Description: " + GetType().Name + $"Bugs fixed : {bugsFixed}, Bugs created : {bugsCreated}");
            }
        }
        public class Janitor : Employee                                         // child inherited class of employee
        {
            public Janitor(string name, int salary) : base(name, salary)        // child class constructor with properties
            {

            }
            public override void OutputEarnings()                               // overriden method of child class
            {
                Console.WriteLine($"Name: {Name}, Salary: {Salary}");           // action of the method
            }
            public override void OutputJobDescription()                         // overriden method of child class
            {
                Console.WriteLine("Job Description: " + GetType().Name);        // action of the method
            }
        }

        public class MaxHeap                            // creating maxheap class
        {
            public Employee[] heap;                     // creating heap array of employee class

            public int GetLeftChild(int index)         // returns the left child of the heap
            {
                return 2 * index + 1;                   
            }
            public int GetRightChild(int index)         // returns the right child of the heap
            {
                return 2 * index + 2;
            }
            public int GetParent(int index)             // returns the parent of the heap
            {
                return (index - 1) / 2;
            }

            public void Swap(int index1, int index2)    // swaps the elements of the heap
            {
                Employee temp = heap[index1];           // creating a temporary employee object
                heap[index1] = heap[index2];            // swapping the elements of the heap
                heap[index2] = temp;                    // swapping the elements of the heap
            }

            public void Insert(Employee employee)       // inserts the elements into the heap
            {
                if (heap == null)                       // if heap is null
                {
                    heap = new Employee[1];             // creating a new heap of size 1
                    heap[0] = employee;                 // assigning the employee object to the heap
                }
                else
                {
                    Employee[] newHeap = new Employee[heap.Length + 1];     // creating a new heap of size heap.Length + 1
                    Array.Copy(heap, newHeap, heap.Length);                 // copying the elements of the heap to the new heap
                    newHeap[newHeap.Length - 1] = employee;                 // assigning the employee object to the new heap
                    heap = newHeap;                                         // assigning the new heap to the heap
                }
                HeapifyUp(heap.Length - 1);                                 // calling the HeapifyUp method to maintain the heap property
            }

            public void HeapifyUp(int index)                                // maintains the heap property
            {
                while (index > 0 && heap[index].Salary > heap[GetParent(index)].Salary) // while loop to check the heap property
                {
                    Swap(index, GetParent(index));                       // swapping the elements of the heap
                    index = GetParent(index);                             // assigning the parent index to the index
                }
            }

            public Employee ExtractMax()                                // delets the max element from the heap
            {
                if (heap == null || heap.Length == 0)                   // if heap is null or heap length is 0
                {
                    return null;                                        // return from the method
                }
                Employee max = heap[0];                      // creating max object of the employee class & assigning to the first element of the heap
                heap[0] = heap[heap.Length - 1];            // assigning the last element of the heap to the first element of the heap
                Array.Resize(ref heap, heap.Length - 1);    // resizing the heap to remove the last element
                HeapifyDown(0);                             // calling the HeapifyDown method to maintain the heap property
                return max;                                 // returning the max object
            }

            public void HeapifyDown(int index)              // maintains the heap property
            {
                int largest = index;                            // assigning the index to the largest variable
                int leftChild = GetLeftChild(index);            // assigning the left child index to the leftChild variable
                int rightChild = GetRightChild(index);          // assigning the right child index to the rightChild variable
                if (leftChild < heap.Length && heap[leftChild].Salary > heap[largest].Salary)
                {                                               // checking if the left child is greater than the largest element
                    largest = leftChild;                        // assigning the left child index to the largest variable
                }
                if (rightChild < heap.Length && heap[rightChild].Salary > heap[largest].Salary)
                {                                               // checking if the right child is greater than the largest element
                    largest = rightChild;                       // assigning the right child index to the largest variable
                }
                if (largest != index)                           // if the largest element is not the index
                {
                    Swap(index, largest);                       // swapping the elements of the heap
                    HeapifyDown(largest);                       // calling the HeapifyDown method to maintain the heap property
                }
            }

            public void PrintHeap()                             // prints the heap
            {
                if (heap == null || heap.Length == 0)           // if heap is null or heap length is 0
                {
                    Console.WriteLine("Heap is empty.");        // displaying message
                    return;                                     // return from the method
                }
                Console.WriteLine("Heap:");                     // displaying message
                for (int i = 0; i < heap.Length; i++)           // for loop to iterate through the heap
                {
                    Console.WriteLine($"Index: {i}, Name: {heap[i].Name}, Salary: {heap[i].Salary}");   // displaying the heap
                }
            }
        }
    }
}
