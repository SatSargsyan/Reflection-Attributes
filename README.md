# Reflection-Attributes

###<a href=https://msdn.microsoft.com/ru-ru/library/aa288454(v=vs.71).aspx>Attributes </a>provide a powerful method of associating declarative information with C# code (types, methods, properties, and so forth). Once associated with a program entity, the attribute can be queried at run time and used in any number of ways.
####Example usage of attributes includes:
<ul>
<li>Associating help documentation with program entities (through a Help attribute).
<li>Associating value editors to a specific type in a GUI framework (through a ValueEditor attribute).
</ul>
####In addition to a complete example, this tutorial includes the following topics:
<ul>
<li>Declaring an Attribute Class
The first thing you need to be able to do is declare an attribute.
<li>Using an Attribute Class
Once the attribute has been created, you then associate the attribute with a particular program element.
<li>Accessing Attributes Through Reflection
Once the attribute has been associated with a program element, you use reflection to query its existence and its value.
</ul>
####Declaring an Attribute Class
Declaring an attribute in C# is simple — it takes the form of a class declaration that inherits from System.Attribute and has been marked with the AttributeUsage attribute as shown below:
```C#
using System;
[AttributeUsage(AttributeTargets.All)]
public class HelpAttribute : System.Attribute 
{
   public readonly string Url;

   public string Topic               // Topic is a named parameter
   {
      get 
      { 
         return topic; 
      }
      set 
      { 

        topic = value; 
      }
   }

   public HelpAttribute(string url)  // url is a positional parameter
   {
      this.Url = url;
   }

   private string topic;
}
```
####Code Discussion
<ul>
<li>The attribute AttributeUsage specifies the language elements to which the attribute can be applied.
<li>Attributes classes are public classes derived from System.Attribute that have at least one public constructor.
<li>Attribute classes have two types of parameters:
<ul>
<li>Positional parameters must be specified every time the attribute is used. Positional parameters are specified as constructor arguments to the attribute class. In the example above, url is a positional parameter.
<li>Named parameters are optional. If they are specified when the attribute is used, the name of the parameter must be used. Named parameters are defined by having a nonstatic field or property. In the example above, Topic is a named parameter.
</ul>
<li>Attribute parameters are restricted to constant values of the following types:
<ul>
<li>Simple types (bool, byte, char, short, int, long, float, and double)
<li>string
<li>System.Type
<li>enums
<li>object (The argument to an attribute parameter of type object must be a constant value of one of the above types.)
<li>One-dimensional arrays of any of the above types
</ul>
</ul>
####Parameters for the AttributeUsage Attribute
The attribute <b>AttributeUsage</b> provides the underlying mechanism by which attributes are declared.
<b>AttributeUsage</b> has one positional parameter:
<ul>
<li><b>AllowOn,</b> which specifies the program elements that the attribute can be assigned to (class, method, property, parameter, and so on). Valid values for this parameter can be found in the<b> System.Attributes.AttributeTargets</b> enumeration in the .NET Framework. The default value for this parameter is all program elements <b>(AttributeElements.All).</b>
</ul>
<b>AttributeUsage</b> has one named parameter:
<ul>
<li>AllowMultiple, a Boolean value that indicates whether multiple attributes can be specified for one program element. The default value for this parameter is False.
</ul>
###Using an Attribute Class
Here's a simple example of using the attribute declared in the previous section:
```C#
[HelpAttribute("http://localhost/MyClassInfo")]
class MyClass 
{
}
```
In this example, the HelpAttribute attribute is associated with MyClass.
####<b>Note</b>   By convention, all attribute names end with the word "Attribute" to distinguish them from other items in the .NET Framework. However, you do not need to specify the attribute suffix when using attributes in code. For example, you can specify HelpAttribute as follows:
```C#
[Help("http://localhost/MyClassInfo")] // [Help] == [HelpAttribute]
class MyClass
{
}
```
###Accessing Attributes Through Reflection
Once attributes have been associated with program elements,<a href=https://msdn.microsoft.com/ru-ru/library/mt656691.aspx> reflection</a> can be used to query their existence and values. The main reflection methods to query attributes are contained in the <b>System.Reflection.MemberInfo</b> class (GetCustomAttributes family of methods). The following example demonstrates the basic way of using reflection to get access to attributes:
```C#
class MainClass 
{
   public static void Main() 
   {
      System.Reflection.MemberInfo info = typeof(MyClass);
      object[] attributes = info.GetCustomAttributes(true);
      for (int i = 0; i < attributes.Length; i ++)
      {
         System.Console.WriteLine(attributes[i]);
      }
   } 
} 
```
Example
The following is a complete example where all pieces are brought together.
