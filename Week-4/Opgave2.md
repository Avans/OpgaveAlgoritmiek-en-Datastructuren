# Week 4
## Week 4.2.1
De opgave is om deze simulatie te maken EN daar gebruik te maken 
van een queue. Hou je aan het OO paradigma: maak bijvoorbeeld een 
aparte Queue–klasse. Breid de Queue–klasse uit met een methode 
getCount() die aangeeft hoeveel elementen zich in de Queue 
bevinden.

_CashRegister.cs_
```C#
class CashRegister
    {
        private int m_CostumerPerTick;
        public Queue<char> Queue { get; set; }

        public CashRegister(int CostumerPerTick)
        {
            m_CostumerPerTick = CostumerPerTick;
            Queue = new Queue<char>();
        }

        public void ProcesQueue()
        {
            for (int i = 0; i < m_CostumerPerTick; i++)
            {
                if (Queue.Count != 0)
                {
                    Queue.Remove();
                }
            }
        }
    }
```

_Queue.cs_
```C#
class Queue<T>
    {
        public LinkedList<T> Data { get; set; }
        public int Count { get; set; }

        public Queue()
        {
            Data = new LinkedList<T>();
        }

        public void Remove()
        {
            Data.RemoveFirst();
            Count--;
        }

        public void Add(T item)
        {
            Data.AddLast(item);
            Count++;
        }

        public int GetCount() // :/
        {
            return Count; 
        }
    }
```

_Simulator.cs_
```C#
class Program
    {
        static void Main(string[] args)
        {
            byte registerCount = 3;
            var sim = new Simulator(registerCount);

            for (int i = 0; i < registerCount; i++)
            {
                int registerSpeed = sim.Generator.Next(1, sim.MaxNewCostumersPerTick + 1);
                sim.Registers[i] = new CashRegister(registerSpeed);
            }

            sim.TimeSteps = 100;
            sim.MaxNewCostumersPerTick = 4;
            sim.runSim();
        }
    }

    class Simulator
    {
        public int TimeSteps { get; set; }
        public CashRegister[] Registers { get; set; }
        public int MaxNewCostumersPerTick { get; set; }
        public Random Generator { get; set; }


        private int m_CurrentTimeStep;

        public Simulator(int registerCount)
        {
            Generator = new Random();
            Registers = new CashRegister[registerCount];
        }

        public void runSim()
        {
            for (m_CurrentTimeStep = 0; m_CurrentTimeStep < TimeSteps; m_CurrentTimeStep++)
            {
                foreach (CashRegister register in Registers)
                {
                    int newCostumerCount = Generator.Next(MaxNewCostumersPerTick);

                    for (int j = 0; j < newCostumerCount; j++)
                    {
                        char costumer = GetRandomLetter();
                        register.Queue.Add(costumer);
                    }

                    register.ProcesQueue();
                }
                printQueues();
                Console.Read();
            }
            Console.Read();
        }

        private void printQueues()
        {
            Console.WriteLine("Timestep: {0}", m_CurrentTimeStep);
            foreach (CashRegister register in Registers)
            {
                Console.Write("Register Queue: ");
                foreach (char costumer in register.Queue.Data)
                {
                    Console.Write(costumer);
                }
                Console.WriteLine();
            }
            Console.WriteLine();
        }

        private char GetRandomLetter()
        {
            int num = Generator.Next(0, 26);
            char let = (char)('a' + num);
            return let;
        }
    }
```

## Week 4.2.2
Run je applicatie en ga na wanneer de rijen bij de kassas erg lang 
worden en wanneer klanten goed geholpen worden.

_Wanneer registerSpeed grooter is dan MaxNewCostumersPerTick verloopt de verwerking soepel. (De rijen worden niet langer)
Indien registerSpeed kleiner of gelijk is aan MaxNewCostumersPerTick dan zal de rij (langzaam) langer worden._

## Week 4.2.1
Lever de broncode in.
```C#
static void Main(string[] args)
        {
            LinkedStack theStack = new LinkedStack();
            string line = "D{I(I{W()})}W()";

            foreach (char item in line)
            {
                switch (item)
                {
                    case 'D':
                    case 'I':
                    case '{':
                    case '[':
                    case '(':
                        theStack.push(item.ToString()); // push them
                        break;
                    case 'E':
                    case 'W':
                    case '}':
                    case ']':
                    case ')':
                        if (!theStack.isEmpty()) // if stack not empty,
                        {
                            char last = theStack.pop()[0]; // pop and check

                            if ((item == '}' && last != '{') ||
                                (item == ']' && last != '[') ||
                                (item == ')' && last != '(') ||
                                (item == 'E' && last != 'I') ||
                                (item == 'W' && last != 'D'))
                            {
                                //'parent' block closed, no else or while found. Remove If en Do.
                                if (last == 'I')
                                    theStack.pop();

                                else if (last == 'D')
                                    theStack.pop();
                                else
                                {
                                    throw new Exception("Error: " + item + " last found:" + last);
                                }

                            }
                        }
                        else // prematurely empty
                            throw new Exception("Error: " + item);
                        break;
                    default: // no action on other characters
                        break;
                } // end switch
            } // end for

            Console.WriteLine("Code is valid");
            Console.Read();
        }
```
