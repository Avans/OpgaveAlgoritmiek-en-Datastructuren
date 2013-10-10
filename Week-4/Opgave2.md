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
_De broncode die we gebruikt hebben voor Week 4 is de zelde stack als we gemaakt hadden voor 4.2._
