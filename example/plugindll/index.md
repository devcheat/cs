# Dynamically Load & Execute dll

The following code shows how can we load a dll to app doamin and execute it dynamically.

## Create the sample `Assembly.dll` by compiling the below code as dll
```cs
//Assembly.dll
namespace TestAssembly{
    public class Main{

        public void Hello()
        { 
            var name = Console.ReadLine();
            Console.WriteLine("Hello() called");
            Console.WriteLine("Hello" + name + " at " + DateTime.Now);
        }

        public void Run(string parameters)
        { 
            Console.WriteLine("Run() called");
            Console.Write("You typed:"  + parameters);
        }

        public string TestNoParameters()
        {
            Console.WriteLine("TestNoParameters() called");
            return ("TestNoParameters() called");
        }

        public void Execute(object[] parameters)
        { 
            Console.WriteLine("Execute() called");
           Console.WriteLine("Number of parameters received: "  + parameters.Length);

           for(int i=0;i<parameters.Length;i++){
               Console.WriteLine(parameters[i]);
           }
        }
    }
}
```

## Create and exeutable console application by compiling the following code
```cs
void Main()
{
	ExecuteWithReflection("Hello"); //invokes the Hellow method
	ExecuteWithReflection("Run","jo"); //passes "jo" as parameter
	ExecuteWithReflection("TestNoParameters"); //Executes TestNoParameters() with no parameters
	ExecuteWithReflection("Execute",new object[]{"jo","wo"});//invokes Execute() with object parameter
}

// Define other methods and classes here

private void ExecuteWithReflection(string methodName,object parameterObject =null)
{
    Assembly assembly = Assembly.LoadFile("C:\\Program Files (x86)\\LINQPad4\\test2.dll");
    Type typeInstance = assembly.GetType("TestAssembly.Main");

    if (typeInstance != null)
    {
        MethodInfo methodInfo = typeInstance.GetMethod(methodName);
        ParameterInfo[] parameterInfo = methodInfo.GetParameters();
        object classInstance = Activator.CreateInstance(typeInstance, null);
		
        if (parameterInfo.Length == 0)
        {
            // there is no parameter we can call with 'null'
            var result = methodInfo.Invoke(classInstance, null);
        }
        else
        {
            var result = methodInfo.Invoke(classInstance,new object[] { parameterObject } );
        }
    }
}
```
