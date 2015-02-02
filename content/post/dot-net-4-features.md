+++
date = "2010-05-16T09:01:44-07:00"

title = "dotNet 4 New Features"
tags = ["c#", "programming", "dotNet"]
categories = ["Software Development"]
+++

.Net 4 has been out for a couple of weeks now and I have started to delve into the new features. Now we are up to C# version 4 as well, and one of the new features that I find really exciting is Optional and Named Parameters. Being able to use optional parameters in functions is something that is built into C++ and until now has been completely missing from C#. In the past, you basically had to override a function several times in order to give the caller more options for leaving parameters out. I am surprised really that it took this long to get them in C#, but I'm glad they are here now! Since overriding functions was the only way to do it in the past, I have to wonder if behind the scenes that is all that Microsoft is doing, generating overrides for functions, perhaps someday I'll look into that. For now let me explain briefly how these work in C# 4.0.

Much like in C++ optional parameters must come after required parameters in the parameter list:

```csharp
void PrintCustomer(string name, string state, string salutation="n/a", string city="", int sales=100)
```

As you can see, it's pretty simple. Just create the parameter as usual, but add the default value to it. So now you can call the function with ONLY the name and state parameters if you want to.

```csharp
PrintCustomer("Bob Smith", "AZ");
```

Pretty cool! Previously you have had to have 4 different overrides of that function in order to accommodate all 3 of those optional parameter configurations. So just like in C++ you can call the function with or without any of the optional parameters, but in C++ you would have to go through each of them in order, you wouldn't for example be able to define city without defining a value for salutation. This is where Named Parameters come into play. With named parameters you can specify the parameter that you want, like this:

```csharp
PrintCustomer("Bob Smith", "AZ", city: "Phoenix");
```

Awesome! Now we have bypassed the need to have a value for salutation! Also it is good to note that you can use named parameters even for the required parameters. That way you can provide parameters to functions in any order that you want, and provide even greater readability in code. Perhaps you have 2 similarly named parameters like state and salesState, with named parameters it would be easy to understand which is which from the call of the function (though it wouldn't be too often you'd be using string literals in your function calls).

Here is a complete VS2010 console application demonstrating this new functionality:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Net4Tests
{
    class Program
    {
        static void Main(string[] args)
        {
            PrintCustomer("Tech Co", "AZ");
            PrintCustomer("ABC Engineering", "CA", "San Diego");
            PrintCustomer("SwizzleSoft", "AZ", zip: "12345");
            PrintCustomer("Bob Smith", city: "Phoenix", state: "AZ");

            Console.ReadLine();
        }

        static void PrintCustomer(string name, string state, string city = "", string zip = "")
        {
            Console.WriteLine("{0}\t{1},{2} {3}", name, city, state, zip);
        }
    }
}
```
