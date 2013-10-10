# Week 4
## Week 4.1
Maak een programma dat eenvoudige XML–teksten valideert. Maak 
hierbij gebruik van een aparte Queue–klasse of Stack–klasse. Geef 
vooral ook aan waarom je welke klasse gebruikt hebt. 
Je mag aannemen dat het kleiner–dan– (<) en groter –dan –teken (>) 
alleen gebruikt worden voor tags. Deze twee tekens staan niet in de 
tekst die tussen de tags staat. 


_Validate.cs_
```C#
class Program
    {
        static void Main(string[] args)
        {
            LinkedStack theStack = new LinkedStack();



            string[] lines = System.IO.File.ReadAllLines(@"D:\ProjectHexagon\AlgoritmiekDatastructuren\AlgoritmiekDatastructuren\XMLFile1.xml");

            foreach (string line in lines)
            {
                bool tag = false;
                string tagName = "";
                foreach (char item in line)
                {
                    if (item == '<')
                    {
                        tag = true;
                        tagName = "";
                    }
                    else if (item == '>' && tagName.Length > 0)
                    {
                        tag = false;
                        if (tagName[0] == '/')
                        {
                            tagName = tagName.Remove(0, 1);
                            if (tagName == theStack.peek())
                            {
                                theStack.pop();
                            }
                            else
                            {
                                throw new Exception("Syntax Error (laatste tag is niet gelijk aan huidige tag)");
                            }
                        }
                        else
                        {
                            theStack.push(tagName);
                        }

                    }
                    else
                    {
                        tagName = tagName + item;
                    }
                }

            }
            if (!theStack.isEmpty())
            {
                throw new Exception("Syntax Error (niet alle tags gesloten) ");
            }
            Console.WriteLine("XML File is valid");
            Console.Read();
        }
    }
```
