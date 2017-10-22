---
title: HW3
---

Homework three was an introduction to C# and its libraries with a postfix calculator. For my project I debated about several different ways to deal with parts of the calculator given that C# does not have any defualt Scanner like Java. I wrote my own given some tutorials found on Stack Overflow. I ended up causing some parse errors that could really just be solved with the built in classes. Instead I just used the base string operations along with double.TryParse to get what I needed done. I did however leave my Scanner class in for reference. Huge thanks to @Stepehn Oliver and @Devon Smith for pointing out mistakes I had made in my code. I didn't realize I had my double conversions backwards and I wasn't pushing the answer to the stack causing incorrect calculations. This was also incorrect in the original code.

I did the usual and created a repo and pushed stuff to it. I have been creating separate repos for each assignment so I can keep track of them later. I already had code repos and wanted to keep them separate as well.

The only thing that didn't really change much from the original java code was the Stack ADT. The only difference was naming convention adding an 'I' in front of the name (convention for interfaces in C#).

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace PostfixCalculator
{
    interface IStackADT
    {
        /**
     * Push an item onto the top of the stack. Pushing an object that 
     * doesn’t exist should result in an error and should not succeed.
     * Pushing an object that is not an item should result in an error.
     * This operation returns a reference (pointer or link, but not a copy)
     * to the item pushed so that an anonymous object can be pushed and then used.
     * @param newItem The object to push onto the top of the stack.  Should not be null
     * @return A reference to the object that was pushed, or null if newItem == null
     */
        object Push(object newItem);

        /**
         * Remove and return the top item on the stack. This operation should 
         * result in an error if the stack is empty. Returns a reference to the 
         * item removed.
         * @return A reference that was popped (and removed) from the stack or null if
         * 			the stack is empty
         */
        object Pop();

        /**
         * Return the top item but do not remove it. Generally should result in 
         * an error if the stack is empty. An acceptable alternative is to return 
         * something which the user can use to check to see if the stack was in fact empty.
         * @return A reference to the item currently on the top of the stack or null if
         * 			the stack is empty
         */
        object Peek { get; }

        /**
         * Query the stack to see if it is empty or not. Cannot produce an error.
         * @return True if the stack is empty, false otherwise
         */
        bool IsEmpty { get; }

        /**
         * Reset the stack by emptying it. The exact technique used to clear 
         * the stack is up to the implementor. The user should pay attention to what 
         * this behavior is.
         */
        void Clear();
    }
}
```

The next class that only changed a little was the Node. C# has quick member assignments for get and set. It also can make use of the statc modifier to allow quick building of the data.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace PostfixCalculator
{
    /**
     * Class Node, used for type generics
     */
    class Node
    {
        //Global variables
        object data;
        Node next;

        /**
         * Default constructor for Node
         */
        public Node()
        {
            data = null;
            next = null;
        }

        /**
         * Constructor for Node
         * input object the data to be input, Node the pointer to the
         * next element
         */
        public Node(object newData, Node newNext)
        {
            data = newData;
            next = newNext;
        }

        /**
        public object Data
        {
            set => data = value;
            get => data;
        }
        */

        //More CSharpish way to do it, previous is also valid
        public object Data { get { return data; } set { data = value; } }

        public Node Next { get { return next; } set { next = value; } }
    }
}
```

The LinkedStack had quite a few changes to allow for standard operations. The other changes were nameing since methods begin with capital letters. The major one was to peek. I used the ? operator to determine if the top was null and if it was return null else return the item. I retrospect I could have done that with the other methods but I left them looking like the original.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace PostfixCalculator
{
    /**
    * A singly linked stack implementation.
    */
    class LinkedStack : IStackADT
    {
        //Global variables
        private Node top;

        //Constructor
        public LinkedStack()
        {
            top = null;
        }

        //Stack Pop method
        public object Pop()
        {
            if (IsEmpty)
            {
                return null;
            }
            object topItem = top;
            top = top.Next;
            return topItem;
        }

        //Stack Peek method
        public object Peek => IsEmpty ? null : top.Data; //returns null if empty and top.Data if it isn't

        //Clear, sets top pointer to null
        public void Clear()
        {
            top = null;
        }

        //IsEmpty, Checks if top is null
        public bool IsEmpty => top == null;

        //Push method, pushes the new object onto the stack
        public object Push(object newItem)
        {
            if (newItem == null)
            {
                return null;
            }
            Node newNode = new Node(newItem, top);
            top = newNode;
            return newItem;
        }
    }
}
```

The Calculator was a little difficult to figure out without the scanner class. I eventually got it to work using the TryParse and stack opertions. I also created my own Scanner class that did the same things and the Java one but ran into boxing issues and eventually abandoned the idea.

```csharp
using System;
using static System.Console;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace PostfixCalculator
{
    /**
     * This class implements a postfix calculator
     * it promts the user for input and returns the result
     */
    class Calculator
    {
        //Global variables
        private LinkedStack stack = new LinkedStack();

        //Main
        static void Main(string[] args)
        {
            Calculator app = new Calculator();
            bool playAgain = true;
            WriteLine("Postfix Calculator.\nRecognizes these operators: +-*/ ");
            while (playAgain)
            {
                playAgain = app.DoCalculation();
            }
            WriteLine("Bye!");
        }

        /**
         * This method promts the user for input and 
         * begins the evaluation
         */
        private bool DoCalculation()
        {
            WriteLine("Enter q to quit\n");
            string input = "2 2 + ";
            WriteLine("> "); //User Prompt

            input = ReadLine();

            if (input.StartsWith("q") || input.StartsWith("Q"))
            {
                return false;
            }

            string output = "4";
            try
            {
                output = EvaluatePostfixInput(input);
                WriteLine("EvaluatePostfixInput");
            }
            catch (ArgumentException e)
            {
                output = e.Message;
            }
            WriteLine("\n>>> " + input + " = " + output);
            return true;
        }

        /*
         * This method pushes the numbers to the stack and 
         * does an operation if it runs into a operator
         * it does the calculation and pushes the result on the stack
         */
        public string EvaluatePostfixInput(string input)
        {
            if (input == null || input == "")
                throw new ArgumentException("Null or the empty string are not valid postfix expressions.");
            // Clear our stack before doing a new calculation
            stack.Clear();

            string token; // will store operator
            double temp; // first value, a 
            double temp2; // temp from a double output.
            double operand; // second value, b
            double answer = 0.0; // accumulator


            string[] inputs = input.Split(' ');

            for (int i = 0; i <= inputs.Length - 1; i++)
            {

                if (double.TryParse(inputs[i], out temp2))
                {
                    stack.Push(temp2);
                }
                else
                {
                    // Is the stack empty?
                    if (stack.IsEmpty)
                    {
                        throw new ArgumentException("Improper input format. Stack became empty when expecting second operand.");
                    }
                    // is the non-numeric item an operator?
                    else if (inputs[i].Length > 1)
                    {
                        throw new ArgumentException("Input Error: " + inputs[i] + " is not an allowed number or operator");
                    }
                    else
                    {
                        // operand a
                        Node nd1 = (Node)stack.Pop();
                        // operand b
                        Node nd2 = (Node)stack.Pop();
                        // operand a
                        temp = (double)nd1.Data;
                        // operand b
                        operand = (double)nd2.Data;
                        // operator
                        token = inputs[i];
                        // get the answer from the DoOperation method.
                        answer = DoOperation(token, temp, operand);
                        // push the answer on to the stack.
                        stack.Push(answer);
                    }

                }
            }
            // return the solution.
            return Convert.ToString(answer);
        }

        /**
         * This method 
         * 
         */
        public double DoOperation(string token, double temp, double operand)
        {
            double output = 0;
            if (token == "+")
            {
                output = temp + operand;
                //WriteLine("Plus");
            }
            else if (token == "-")
            {
                output = (operand - temp);
                //WriteLine("Minus");
            }
            else if (token == "*")
            {
                output = temp * operand;
                //WriteLine("Multiply");
            }
            else if (token == "/")
            {
                //WriteLine("Divide");
                try
                {
                    output = (operand / temp);
                    if (output == Double.NegativeInfinity || output == Double.PositiveInfinity)
                        throw new ArgumentException("Can't divide by zero");
                }
                catch (ArithmeticException e)
                {
                    throw new ArgumentException(e.ToString());
                }
            }
            else
            {
                throw new ArgumentException("Improper operator: " + token + ", is not one of +, -, *, or /");
            }

            return output;
        }

        //public string EvaluatePostfixInput(string input)
        //{
        //    if (input == null || input == "")
        //        throw new ArgumentException("Null or the empty string are not valid postfix expressions.");
        //    // Clear our stack before doing a new calculation
        //    stack.Clear();

        //    String s;   // Temporary variable for token read
        //    double a;   // Temporary variable for operand
        //    double b;   // ...for operand
        //    double c;   // ...for answer

        //    Scanner st = new Scanner(input);
        //    while (st.hasNext())
        //    {
        //        if (st.hasNextDouble())
        //        {
        //            stack.Push(Convert.ToDouble(st.nextDouble()));    // if it's a number push it on the stack
        //        }
        //        else
        //        {
        //            // Must be an operator or some other character or word.
        //            if (st.hasNext())
        //            {
        //                s = st.next(); 
        //            }
        //            else
        //            {
        //                break;
        //            }
        //            if (s?.Length > 1)
        //            {
        //                throw new ArgumentException("Input Error: " + s + " is not an allowed number or operator");
        //            }
        //            // it may be an operator so pop two values off the stack and perform the indicated operation
        //            if (stack.IsEmpty)
        //            {
        //                throw new ArgumentException("Improper input format. Stack became empty when expecting second operand.");
        //            }

        //            b = (double)stack.Pop();
        //            if (stack.IsEmpty)
        //                throw new ArgumentException("Improper input format. Stack became empty when expecting first operand.");
        //            a = Convert.ToDouble(stack.Pop());
        //            // Wrap up all operations in a single method, easy to add other binary operators this way if desired
        //            c = DoOperation(a, b, s);
        //            // push the answer back on the stack
        //            stack.Push(Convert.ToDouble(c));
        //        }
        //    }// End while
        //    return stack.Pop().ToString();
        //}

        //public double DoOperation(double a, double b, String s)
        //{
        //    double c = 0.0D;
        //    if (s == "+")      // Can't use a switch-case with Strings, so we do if-else
        //        c = (a + b);
        //    else if (s == "-")
        //        c = (a - b);
        //    else if (s == "*")
        //        c = (a * b);
        //    else if (s == "/")
        //    {
        //        try
        //        {
        //            c = (a / b);
        //            if (c == double.NegativeInfinity || c == double.PositiveInfinity)
        //                throw new ArgumentException("Can't divide by zero");
        //        }
        //        catch (ArithmeticException e)
        //        {
        //            throw new ArgumentException(e.ToString());
        //        }
        //    }
        //    else
        //        throw new ArgumentException("Improper operator: " + s + ", is not one of +, -, *, or /");

        //    return c;
        //}
    }
}
```

Here is the Scanner class I wrote:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

//Original idea from https://stackoverflow.com/questions/722270/is-there-an-equivalent-to-the-scanner-class-in-c-sharp-for-strings
namespace PostfixCalculator
{
    /**
     * This class is similar to the Java util.Scanner class.
     * It tokenizes a string and returns a sub section of the string
     */
    class Scanner : System.IO.StringReader
    {
        //Global variables
        private string currentWord;

        /**
         * Constructor for Scanner
         */
        public Scanner(string source) : base(source)
        {
            readNextWord();
        }

        /**
         * readNextWord, reads the next word in the string
         * Sets the global variable to the string value pointed at.
         */
        private void readNextWord()
        {
            System.Text.StringBuilder sb = new StringBuilder();
            char nextChar;
            int next;
            do
            {
                next = this.Read();
                if (next < 0)
                {
                    break;
                }

                nextChar = (char)next;
                if (char.IsWhiteSpace(nextChar))
                {
                    break;
                }

                sb.Append(nextChar);
            } while (true);
            while ((this.Peek() >= 0) && (char.IsWhiteSpace((char)this.Peek())))
            {
                this.Read();
            }

            if (sb.Length > 0)
            {
                currentWord = sb.ToString();
            }
            else
            {
                currentWord = null;
            }
        }

        /**
         * hasNextInt return a bool if the next value is an int
         */
        public bool hasNextInt()
        {
            if (currentWord == null)
            {
                return false;
            }

            int dummy;
            return int.TryParse(currentWord, out dummy);
        }

        /**
         * nextInt returns the next integer token in the input string
         */
        public int nextInt()
        {
            try
            {
                return int.Parse(currentWord);
            }
            finally
            {
                readNextWord();
            }
        }

        /**
         * hasNextDouble returns a a bool if the next value is a double
         */
        public bool hasNextDouble()
        {
            if (currentWord == null)
            {
                return false;
            }
            double dummy;
            return double.TryParse(currentWord, out dummy);
        }

        /**
         * nextDouble returns the nexe double token in the input string
         */
        public double nextDouble()
        {
            try
            {
                return double.Parse(currentWord);
            }
            finally
            {
                readNextWord();
            }
        }

        /**
         * hasNext returns a bool if there is or isn't another token in the input string
         */
        public bool hasNext()
        {
            return currentWord != null;
        }

        /**
         * next reutrns the next string token in the input string
         */
        public string next()
        {
            if (!hasNext())
            {
                readNextWord();
                return currentWord;
            }
            else
            {
                return null;
            }
        }
    }
}
```

### Screenshots of the calculator in operation
----

![First calculation 2 2 +](/img/pfcalc1.png)

![Second calculation(complex) 15 7 1 1 + - / 3 * 2 1 1 + +](/img/pfcalc2.png)