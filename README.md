# Clean Code 

If you liked `clean-code-dotnet` project or if it helped you, please give a star :star: for this repository. That will not only help strengthen our .NET community but also improve skills about the clean code for .NET developers in around the world. Thank you very much :+1:



# Table of Contents


- [Introduction](#introduction)
- [Clean Code Tips](#clean-code-net)
  - [Naming](#naming)
  - [Variables](#variables)
  - [Functions](#functions)
  - [Objects and Data Structures](#objects-and-data-structures)
  - [Classes](#classes)
 


# Introduction

Some Rule for Cleancode



# Clean Code .NET

## Naming

<details>
  <summary><b>Avoid using bad names</b></summary>
A good name allows the code to be used by many developers. The name should reflect what it does and give context.

**Bad:**

```csharp
int d;
```

**Good:**

```csharp
int daySinceModification;
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid Misleading Names</b></summary>

Name the variable to reflect what it is used for.

**Bad:**

```csharp
var dataFromDb = db.GetFromService().ToList();
```

**Good:**

```csharp
var listOfEmployee = _employeeService.GetEmployees().ToList();
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid Hungarian notation</b></summary>

Hungarian Notation restates the type which is already present in the declaration. This is pointless since modern IDEs will identify the type.

**Bad:**

```csharp
int iCounter;
string strFullName;
DateTime dModifiedDate;
```

**Good:**

```csharp
int counter;
string fullName;
DateTime modifiedDate;
```

Hungarian Notation should also not be used in paramaters.

**Bad:**

```csharp
public bool IsShopOpen(string pDay, int pAmount)
{
    // some logic
}
```

**Good:**

```csharp
public bool IsShopOpen(string day, int amount)
{
    // some logic
}
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use consistent capitalization</b></summary>

Capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Bad:**

```csharp
const int DAYS_IN_WEEK = 7;
const int daysInMonth = 30;

var songs = new List<string> { 'Back In Black', 'Stairway to Heaven', 'Hey Jude' };
var Artists = new List<string> { 'ACDC', 'Led Zeppelin', 'The Beatles' };

bool EraseDatabase() {}
bool Restore_database() {}

class animal {}
class Alpaca {}
```

**Good:**

```csharp
const int DaysInWeek = 7;
const int DaysInMonth = 30;

var songs = new List<string> { 'Back In Black', 'Stairway to Heaven', 'Hey Jude' };
var artists = new List<string> { 'ACDC', 'Led Zeppelin', 'The Beatles' };

bool EraseDatabase() {}
bool RestoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use pronounceable names</b></summary>

It will take time to investigate the meaning of the variables and functions when they are not pronounceable.

**Bad:**

```csharp
public class Employee
{
    public Datetime sWorkDate { get; set; } // what the heck is this
    public Datetime modTime { get; set; } // same here
}
```

**Good:**

```csharp
public class Employee
{
    public Datetime StartWorkingDate { get; set; }
    public Datetime ModificationTime { get; set; }
}
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use Camelcase notation</b></summary>

Use [Camelcase Notation](https://en.wikipedia.org/wiki/Camel_case) for variable and method paramaters.

**Bad:**

```csharp
var employeephone;

public double CalculateSalary(int workingdays, int workinghours)
{
    // some logic
}
```

**Good:**

```csharp
var employeePhone;

public double CalculateSalary(int workingDays, int workingHours)
{
    // some logic
}
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use domain name</b></summary>

People who read your code are also programmers. Naming things right will help everyone be on the same page. We don't want to take time to explain to everyone what a variable or function is for.

**Good**

```csharp
public class SingleObject
{
    // create an object of SingleObject
    private static SingleObject _instance = new SingleObject();

    // make the constructor private so that this class cannot be instantiated
    private SingleObject() {}

    // get the only object available
    public static SingleObject GetInstance()
    {
        return _instance;
    }

    public string ShowMessage()
    {
        return "Hello World!";
    }
}

public static void main(String[] args)
{
    // illegal construct
    // var object = new SingleObject();

    // Get the only object available
    var singletonObject = SingleObject.GetInstance();

    // show the message
    singletonObject.ShowMessage();
}
```

**[⬆ Back to top](#table-of-contents)**

</details>

## Variables

<details>
  <summary><b>Avoid nesting too deeply and return early</b></summary>

Too many if else statements can make the code hard to follow. **Explicit is better than implicit**.

**Bad:**

```csharp
public bool IsShopOpen(string day)
{
    if (!string.IsNullOrEmpty(day))
    {
        day = day.ToLower();
        if (day == "friday")
        {
            return true;
        }
        else if (day == "saturday")
        {
            return true;
        }
        else if (day == "sunday")
        {
            return true;
        }
        else
        {
            return false;
        }
    }
    else
    {
        return false;
    }

}
```

**Good:**

```csharp
public bool IsShopOpen(string day)
{
    if (string.IsNullOrEmpty(day))
    {
        return false;
    }

    var openingDays = new[] { "friday", "saturday", "sunday" };
    return openingDays.Any(d => d == day.ToLower());
}
```

**Bad:**

```csharp
public long Fibonacci(int n)
{
    if (n < 50)
    {
        if (n != 0)
        {
            if (n != 1)
            {
                return Fibonacci(n - 1) + Fibonacci(n - 2);
            }
            else
            {
                return 1;
            }
        }
        else
        {
            return 0;
        }
    }
    else
    {
        throw new System.Exception("Not supported");
    }
}
```

**Good:**

```csharp
public long Fibonacci(int n)
{
    if (n == 0)
    {
        return 0;
    }

    if (n == 1)
    {
        return 1;
    }

    if (n > 50)
    {
        throw new System.Exception("Not supported");
    }

    return Fibonacci(n - 1) + Fibonacci(n - 2);
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid mental mapping</b></summary>

Don’t force the reader of your code to translate what the variable means. **Explicit is better than implicit**.

**Bad:**

```csharp
var l = new[] { "Austin", "New York", "San Francisco" };

for (var i = 0; i < l.Count(); i++)
{
    var li = l[i];
    DoStuff();
    DoSomeOtherStuff();

    // ...
    // ...
    // ...
    // Wait, what is `li` for again?
    Dispatch(li);
}
```

**Good:**

```csharp
var locations = new[] { "Austin", "New York", "San Francisco" };

foreach (var location in locations)
{
    DoStuff();
    DoSomeOtherStuff();

    // ...
    // ...
    // ...
    Dispatch(location);
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid magic string</b></summary>

Magic strings are string values that are specified directly within application code that have an impact on the application’s behavior. Frequently, such strings will end up being duplicated within the system, and since they cannot automatically be updated using refactoring tools, they become a common source of bugs when changes are made to some strings but not others.

**Bad**

```csharp
if (userRole == "Admin")
{
    // logic in here
}
```

**Good**

```csharp
const string ADMIN_ROLE = "Admin"
if (userRole == ADMIN_ROLE)
{
    // logic in here
}
```

Using this we only have to change in centralize place and others will adapt it.

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Don't add unneeded context</b></summary>

If your class/object name tells you something, don't repeat that in your variable name.

**Bad:**

```csharp
public class Car
{
    public string CarMake { get; set; }
    public string CarModel { get; set; }
    public string CarColor { get; set; }

    //...
}
```

**Good:**

```csharp
public class Car
{
    public string Make { get; set; }
    public string Model { get; set; }
    public string Color { get; set; }

    //...
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use meaningful and pronounceable variable names</b></summary>

**Bad:**

```csharp
var ymdstr = DateTime.UtcNow.ToString("MMMM dd, yyyy");
```

**Good:**

```csharp
var currentDate = DateTime.UtcNow.ToString("MMMM dd, yyyy");
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use the same vocabulary for the same type of variable</b></summary>

**Bad:**

```csharp
GetUserInfo();
GetUserData();
GetUserRecord();
GetUserProfile();
```

**Good:**

```csharp
GetUser();
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use searchable names (part 1)</b></summary>

We will read more code than we will ever write. It's important that the code we do write is readable and searchable. By _not_ naming variables that end up being meaningful for understanding our program, we hurt our readers. Make your names searchable.

**Bad:**

```csharp
// What the heck is data for?
var data = new { Name = "John", Age = 42 };

var stream1 = new MemoryStream();
var ser1 = new DataContractJsonSerializer(typeof(object));
ser1.WriteObject(stream1, data);

stream1.Position = 0;
var sr1 = new StreamReader(stream1);
Console.Write("JSON form of Data object: ");
Console.WriteLine(sr1.ReadToEnd());
```

**Good:**

```csharp
var person = new Person
{
    Name = "John",
    Age = 42
};

var stream2 = new MemoryStream();
var ser2 = new DataContractJsonSerializer(typeof(Person));
ser2.WriteObject(stream2, data);

stream2.Position = 0;
var sr2 = new StreamReader(stream2);
Console.Write("JSON form of Data object: ");
Console.WriteLine(sr2.ReadToEnd());
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use searchable names (part 2)</b></summary>

**Bad:**

```csharp
var data = new { Name = "John", Age = 42, PersonAccess = 4};

// What the heck is 4 for?
if (data.PersonAccess == 4)
{
    // do edit ...
}
```

**Good:**

```csharp
public enum PersonAccess : int
{
    ACCESS_READ = 1,
    ACCESS_CREATE = 2,
    ACCESS_UPDATE = 4,
    ACCESS_DELETE = 8
}

var person = new Person
{
    Name = "John",
    Age = 42,
    PersonAccess= PersonAccess.ACCESS_CREATE
};

if (person.PersonAccess == PersonAccess.ACCESS_UPDATE)
{
    // do edit ...
}
```

**[⬆ Back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use explanatory variables</b></summary>

**Bad:**

```csharp
const string Address = "One Infinite Loop, Cupertino 95014";
var cityZipCodeRegex = @"/^[^,\]+[,\\s]+(.+?)\s*(\d{5})?$/";
var matches = Regex.Matches(Address, cityZipCodeRegex);
if (matches[0].Success == true && matches[1].Success == true)
{
    SaveCityZipCode(matches[0].Value, matches[1].Value);
}
```

**Good:**

Decrease dependence on regex by naming subpatterns.

```csharp
const string Address = "One Infinite Loop, Cupertino 95014";
var cityZipCodeWithGroupRegex = @"/^[^,\]+[,\\s]+(?<city>.+?)\s*(?<zipCode>\d{5})?$/";
var matchesWithGroup = Regex.Match(Address, cityZipCodeWithGroupRegex);
var cityGroup = matchesWithGroup.Groups["city"];
var zipCodeGroup = matchesWithGroup.Groups["zipCode"];
if(cityGroup.Success == true && zipCodeGroup.Success == true)
{
    SaveCityZipCode(cityGroup.Value, zipCodeGroup.Value);
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Use default arguments instead of short circuiting or conditionals</b></summary>

**Not good:**

This is not good because `breweryName` can be `NULL`.

This opinion is more understandable than the previous version, but it better controls the value of the variable.

```csharp
public void CreateMicrobrewery(string name = null)
{
    var breweryName = !string.IsNullOrEmpty(name) ? name : "Hipster Brew Co.";
    // ...
}
```

**Good:**

```csharp
public void CreateMicrobrewery(string breweryName = "Hipster Brew Co.")
{
    // ...
}
```

**[⬆ back to top](#table-of-contents)**

</details>

## Functions

<details>
  <summary><b>Avoid side effects</b></summary>

A function produces a side effect if it does anything other than take a value in and return another value or values. A side effect could be writing to a file, modifying some global variable, or accidentally wiring all your money to a stranger.

Now, you do need to have side effects in a program on occasion. Like the previous example, you might need to write to a file. What you want to do is to centralize where you are doing this. Don't have several functions and classes that write to a particular file. Have one service that does it. One and only one.

The main point is to avoid common pitfalls like sharing state between objects without any structure, using mutable data types that can be written to by anything, and not centralizing where your side effects occur. If you can do this, you will be happier
than the vast majority of other programmers.

**Bad:**

```csharp
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
var name = "Ryan McDermott";

public void SplitAndEnrichFullName()
{
    var temp = name.Split(" ");
    name = $"His first name is {temp[0]}, and his last name is {temp[1]}"; // side effect
}

SplitAndEnrichFullName();

Console.WriteLine(name); // His first name is Ryan, and his last name is McDermott
```

**Good:**

```csharp
public string SplitAndEnrichFullName(string name)
{
    var temp = name.Split(" ");
    return $"His first name is {temp[0]}, and his last name is {temp[1]}";
}

var name = "Ryan McDermott";
var fullName = SplitAndEnrichFullName(name);

Console.WriteLine(name); // Ryan McDermott
Console.WriteLine(fullName); // His first name is Ryan, and his last name is McDermott
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid negative conditionals</b></summary>

**Bad:**

```csharp
public bool IsDOMNodeNotPresent(string node)
{
    // ...
}

if (!IsDOMNodeNotPresent(node))
{
    // ...
}
```

**Good:**

```csharp
public bool IsDOMNodePresent(string node)
{
    // ...
}

if (IsDOMNodePresent(node))
{
    // ...
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid conditionals</b></summary>

This seems like an impossible task. Upon first hearing this, most people say, "how am I supposed to do anything without an `if` statement?" The answer is that you can use polymorphism to achieve the same task in many cases. The second question is usually, "well that's great but why would I want to do that?" The answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you are telling your user that your function does more than one thing. Remember, just do one thing.

**Bad:**

```csharp
class Airplane
{
    // ...

    public double GetCruisingAltitude()
    {
        switch (_type)
        {
            case '777':
                return GetMaxAltitude() - GetPassengerCount();
            case 'Air Force One':
                return GetMaxAltitude();
            case 'Cessna':
                return GetMaxAltitude() - GetFuelExpenditure();
        }
    }
}
```

**Good:**

```csharp
interface IAirplane
{
    // ...

    double GetCruisingAltitude();
}

class Boeing777 : IAirplane
{
    // ...

    public double GetCruisingAltitude()
    {
        return GetMaxAltitude() - GetPassengerCount();
    }
}

class AirForceOne : IAirplane
{
    // ...

    public double GetCruisingAltitude()
    {
        return GetMaxAltitude();
    }
}

class Cessna : IAirplane
{
    // ...

    public double GetCruisingAltitude()
    {
        return GetMaxAltitude() - GetFuelExpenditure();
    }
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid type-checking (part 1)</b></summary>

**Bad:**

```csharp
public Path TravelToTexas(object vehicle)
{
    if (vehicle.GetType() == typeof(Bicycle))
    {
        (vehicle as Bicycle).PeddleTo(new Location("texas"));
    }
    else if (vehicle.GetType() == typeof(Car))
    {
        (vehicle as Car).DriveTo(new Location("texas"));
    }
}
```

**Good:**

```csharp
public Path TravelToTexas(Traveler vehicle)
{
    vehicle.TravelTo(new Location("texas"));
}
```

or

```csharp
// pattern matching
public Path TravelToTexas(object vehicle)
{
    if (vehicle is Bicycle bicycle)
    {
        bicycle.PeddleTo(new Location("texas"));
    }
    else if (vehicle is Car car)
    {
        car.DriveTo(new Location("texas"));
    }
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid type-checking (part 2)</b></summary>

**Bad:**

```csharp
public int Combine(dynamic val1, dynamic val2)
{
    int value;
    if (!int.TryParse(val1, out value) || !int.TryParse(val2, out value))
    {
        throw new Exception('Must be of type Number');
    }

    return val1 + val2;
}
```

**Good:**

```csharp
public int Combine(int val1, int val2)
{
    return val1 + val2;
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Avoid flags in method parameters</b></summary>

A flag indicates that the method has more than one responsibility. It is best if the method only has a single responsibility. Split the method into two if a boolean parameter adds multiple responsibilities to the method.

**Bad:**

```csharp
public void CreateFile(string name, bool temp = false)
{
    if (temp)
    {
        Touch("./temp/" + name);
    }
    else
    {
        Touch(name);
    }
}
```

**Good:**

```csharp
public void CreateFile(string name)
{
    Touch(name);
}

public void CreateTempFile(string name)
{
    Touch("./temp/"  + name);
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Don't write to global functions</b></summary>

Polluting globals is a bad practice in many languages because you could clash with another library and the user of your API would be none-the-wiser until they get an exception in production. Let's think about an example: what if you wanted to have configuration array.
You could write global function like `Config()`, but it could clash with another library that tried to do the same thing.

**Bad:**

```csharp
public Dictionary<string, string> Config()
{
    return new Dictionary<string,string>(){
        ["foo"] = "bar"
    };
}
```

**Good:**

```csharp
class Configuration
{
    private Dictionary<string, string> _configuration;

    public Configuration(Dictionary<string, string> configuration)
    {
        _configuration = configuration;
    }

    public string[] Get(string key)
    {
        return _configuration.ContainsKey(key) ? _configuration[key] : null;
    }
}
```

Load configuration and create instance of `Configuration` class

```csharp
var configuration = new Configuration(new Dictionary<string, string>() {
    ["foo"] = "bar"
});
```

And now you must use instance of `Configuration` in your application.

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Don't use a Singleton pattern</b></summary>

Singleton is an [anti-pattern](https://en.wikipedia.org/wiki/Singleton_pattern). Paraphrased from Brian Button:

1. They are generally used as a **global instance**, why is that so bad? Because **you hide the dependencies** of your application in your code, instead of exposing them through the interfaces. Making something global to avoid passing it around is a [code smell](https://en.wikipedia.org/wiki/Code_smell).
2. They violate the [single responsibility principle](#single-responsibility-principle-srp): by virtue of the fact that **they control their own creation and lifecycle**.
3. They inherently cause code to be tightly [coupled](https://en.wikipedia.org/wiki/Coupling_%28computer_programming%29). This makes faking them out under **test rather difficult** in many cases.
4. They carry state around for the lifetime of the application. Another hit to testing since **you can end up with a situation where tests need to be ordered** which is a big no for unit tests. Why? Because each unit test should be independent from the other.

There is also very good thoughts by [Misko Hevery](http://misko.hevery.com/about/) about the [root of problem](http://misko.hevery.com/2008/08/25/root-cause-of-singletons/).

**Bad:**

```csharp
class DBConnection
{
    private static DBConnection _instance;

    private DBConnection()
    {
        // ...
    }

    public static GetInstance()
    {
        if (_instance == null)
        {
            _instance = new DBConnection();
        }

        return _instance;
    }

    // ...
}

var singleton = DBConnection.GetInstance();
```

**Good:**

```csharp
class DBConnection
{
    public DBConnection(IOptions<DbConnectionOption> options)
    {
        // ...
    }

    // ...
}
```

Create instance of `DBConnection` class and configure it with [Option pattern](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-2.1).

```csharp
var options = <resolve from IOC>;
var connection = new DBConnection(options);
```

And now you must use instance of `DBConnection` in your application.

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Function arguments (2 or fewer ideally)</b></summary>

Limiting the amount of function parameters is incredibly important because it makes testing your function easier. Having more than three leads to a combinatorial explosion where you have to test tons of different cases with each separate argument.

Zero arguments is the ideal case. One or two arguments is ok, and three should be avoided. Anything more than that should be consolidated. Usually, if you have more than two arguments then your function is trying to do too much. In cases where it's not, most of the time a higher-level object will suffice as an argument.

**Bad:**

```csharp
public void CreateMenu(string title, string body, string buttonText, bool cancellable)
{
    // ...
}
```

**Good:**

```csharp
public class MenuConfig
{
    public string Title { get; set; }
    public string Body { get; set; }
    public string ButtonText { get; set; }
    public bool Cancellable { get; set; }
}

var config = new MenuConfig
{
    Title = "Foo",
    Body = "Bar",
    ButtonText = "Baz",
    Cancellable = true
};

public void CreateMenu(MenuConfig config)
{
    // ...
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Functions should do one thing</b></summary>

This is by far the most important rule in software engineering. When functions do more than one thing, they are harder to compose, test, and reason about. When you can isolate a function to just one action, they can be refactored easily and your code will read much
cleaner. If you take nothing else away from this guide other than this, you'll be ahead of many developers.

**Bad:**

```csharp
public void SendEmailToListOfClients(string[] clients)
{
    foreach (var client in clients)
    {
        var clientRecord = db.Find(client);
        if (clientRecord.IsActive())
        {
            Email(client);
        }
    }
}
```

**Good:**

```csharp
public void SendEmailToListOfClients(string[] clients)
{
    var activeClients = GetActiveClients(clients);
    // Do some logic
}

public List<Client> GetActiveClients(string[] clients)
{
    return db.Find(clients).Where(s => s.Status == "Active");
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Function names should say what they do</b></summary>

**Bad:**

```csharp
public class Email
{
    //...

    public void Handle()
    {
        SendMail(this._to, this._subject, this._body);
    }
}

var message = new Email(...);
// What is this? A handle for the message? Are we writing to a file now?
message.Handle();
```

**Good:**

```csharp
public class Email
{
    //...

    public void Send()
    {
        SendMail(this._to, this._subject, this._body);
    }
}

var message = new Email(...);
// Clear and obvious
message.Send();
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Functions should only be one level of abstraction</b></summary>

> Not finished yet

When you have more than one level of abstraction your function is usually doing too much. Splitting up functions leads to reusability and easier testing.

**Bad:**

```csharp
public string ParseBetterJSAlternative(string code)
{
    var regexes = [
        // ...
    ];

    var statements = explode(" ", code);
    var tokens = new string[] {};
    foreach (var regex in regexes)
    {
        foreach (var statement in statements)
        {
            // ...
        }
    }

    var ast = new string[] {};
    foreach (var token in tokens)
    {
        // lex...
    }

    foreach (var node in ast)
    {
        // parse...
    }
}
```

**Bad too:**

We have carried out some of the functionality, but the `ParseBetterJSAlternative()` function is still very complex and not testable.

```csharp
public string Tokenize(string code)
{
    var regexes = new string[]
    {
        // ...
    };

    var statements = explode(" ", code);
    var tokens = new string[] {};
    foreach (var regex in regexes)
    {
        foreach (var statement in statements)
        {
            tokens[] = /* ... */;
        }
    }

    return tokens;
}

public string Lexer(string[] tokens)
{
    var ast = new string[] {};
    foreach (var token in tokens)
    {
        ast[] = /* ... */;
    }

    return ast;
}

public string ParseBetterJSAlternative(string code)
{
    var tokens = Tokenize(code);
    var ast = Lexer(tokens);
    foreach (var node in ast)
    {
        // parse...
    }
}
```

**Good:**

The best solution is move out the dependencies of `ParseBetterJSAlternative()` function.

```csharp
class Tokenizer
{
    public string Tokenize(string code)
    {
        var regexes = new string[] {
            // ...
        };

        var statements = explode(" ", code);
        var tokens = new string[] {};
        foreach (var regex in regexes)
        {
            foreach (var statement in statements)
            {
                tokens[] = /* ... */;
            }
        }

        return tokens;
    }
}

class Lexer
{
    public string Lexify(string[] tokens)
    {
        var ast = new[] {};
        foreach (var token in tokens)
        {
            ast[] = /* ... */;
        }

        return ast;
    }
}

class BetterJSAlternative
{
    private string _tokenizer;
    private string _lexer;

    public BetterJSAlternative(Tokenizer tokenizer, Lexer lexer)
    {
        _tokenizer = tokenizer;
        _lexer = lexer;
    }

    public string Parse(string code)
    {
        var tokens = _tokenizer.Tokenize(code);
        var ast = _lexer.Lexify(tokens);
        foreach (var node in ast)
        {
            // parse...
        }
    }
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Function callers and callees should be close</b></summary>

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee. We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Bad:**

```csharp
class PerformanceReview
{
    private readonly Employee _employee;

    public PerformanceReview(Employee employee)
    {
        _employee = employee;
    }

    private IEnumerable<PeersData> LookupPeers()
    {
        return db.lookup(_employee, 'peers');
    }

    private ManagerData LookupManager()
    {
        return db.lookup(_employee, 'manager');
    }

    private IEnumerable<PeerReviews> GetPeerReviews()
    {
        var peers = LookupPeers();
        // ...
    }

    public PerfReviewData PerfReview()
    {
        GetPeerReviews();
        GetManagerReview();
        GetSelfReview();
    }

    public ManagerData GetManagerReview()
    {
        var manager = LookupManager();
    }

    public EmployeeData GetSelfReview()
    {
        // ...
    }
}

var  review = new PerformanceReview(employee);
review.PerfReview();
```

**Good:**

```csharp
class PerformanceReview
{
    private readonly Employee _employee;

    public PerformanceReview(Employee employee)
    {
        _employee = employee;
    }

    public PerfReviewData PerfReview()
    {
        GetPeerReviews();
        GetManagerReview();
        GetSelfReview();
    }

    private IEnumerable<PeerReviews> GetPeerReviews()
    {
        var peers = LookupPeers();
        // ...
    }

    private IEnumerable<PeersData> LookupPeers()
    {
        return db.lookup(_employee, 'peers');
    }

    private ManagerData GetManagerReview()
    {
        var manager = LookupManager();
        return manager;
    }

    private ManagerData LookupManager()
    {
        return db.lookup(_employee, 'manager');
    }

    private EmployeeData GetSelfReview()
    {
        // ...
    }
}

var review = new PerformanceReview(employee);
review.PerfReview();
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Encapsulate conditionals</b></summary>

**Bad:**

```csharp
if (article.state == "published")
{
    // ...
}
```

**Good:**

```csharp
if (article.IsPublished())
{
    // ...
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Remove dead code</b></summary>

Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase. If it's not being called, get rid of it! It will still be safe in your version history if you still need it.

**Bad:**

```csharp
public void OldRequestModule(string url)
{
    // ...
}

public void NewRequestModule(string url)
{
    // ...
}

var request = NewRequestModule(requestUrl);
InventoryTracker("apples", request, "www.inventory-awesome.io");
```

**Good:**

```csharp
public void RequestModule(string url)
{
    // ...
}

var request = RequestModule(requestUrl);
InventoryTracker("apples", request, "www.inventory-awesome.io");
```

**[⬆ back to top](#table-of-contents)**

</details>

## Objects and Data Structures

<details>
  <summary><b>Use getters and setters</b></summary>

In C# / VB.NET you can set `public`, `protected` and `private` keywords for methods.
Using it, you can control properties modification on an object.

- When you want to do more beyond getting an object property, you don't have to look up and change every accessor in your codebase.
- Makes adding validation simple when doing a `set`.
- Encapsulates the internal representation.
- Easy to add logging and error handling when getting and setting.
- Inheriting this class, you can override default functionality.
- You can lazy load your object's properties, let's say getting it from a server.

Additionally, this is part of Open/Closed principle, from object-oriented design principles.

**Bad:**

```csharp
class BankAccount
{
    public double Balance = 1000;
}

var bankAccount = new BankAccount();

// Fake buy shoes...
bankAccount.Balance -= 100;
```

**Good:**

```csharp
class BankAccount
{
    private double _balance = 0.0D;

    pubic double Balance {
        get {
            return _balance;
        }
    }

    public BankAccount(balance = 1000)
    {
       _balance = balance;
    }

    public void WithdrawBalance(int amount)
    {
        if (amount > _balance)
        {
            throw new Exception('Amount greater than available balance.');
        }

        _balance -= amount;
    }

    public void DepositBalance(int amount)
    {
        _balance += amount;
    }
}

var bankAccount = new BankAccount();

// Buy shoes...
bankAccount.WithdrawBalance(price);

// Get balance
balance = bankAccount.Balance;
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Make objects have private/protected members</b></summary>

**Bad:**

```csharp
class Employee
{
    public string Name { get; set; }

    public Employee(string name)
    {
        Name = name;
    }
}

var employee = new Employee("John Doe");
Console.WriteLine(employee.Name); // Employee name: John Doe
```

**Good:**

```csharp
class Employee
{
    public string Name { get; }

    public Employee(string name)
    {
        Name = name;
    }
}

var employee = new Employee("John Doe");
Console.WriteLine(employee.Name); // Employee name: John Doe
```

**[⬆ back to top](#table-of-contents)**

</details>

## Classes

<details>
  <summary><b>Use method chaining</b></summary>

This pattern is very useful and commonly used in many libraries. It allows your code to be expressive, and less verbose.
For that reason, use method chaining and take a look at how clean your code will be.

**Good:**

```csharp
public static class ListExtensions
{
    public static List<T> FluentAdd<T>(this List<T> list, T item)
    {
        list.Add(item);
        return list;
    }

    public static List<T> FluentClear<T>(this List<T> list)
    {
        list.Clear();
        return list;
    }

    public static List<T> FluentForEach<T>(this List<T> list, Action<T> action)
    {
        list.ForEach(action);
        return list;
    }

    public static List<T> FluentInsert<T>(this List<T> list, int index, T item)
    {
        list.Insert(index, item);
        return list;
    }

    public static List<T> FluentRemoveAt<T>(this List<T> list, int index)
    {
        list.RemoveAt(index);
        return list;
    }

    public static List<T> FluentReverse<T>(this List<T> list)
    {
        list.Reverse();
        return list;
    }
}

internal static void ListFluentExtensions()
{
    var list = new List<int>() { 1, 2, 3, 4, 5 }
        .FluentAdd(1)
        .FluentInsert(0, 0)
        .FluentRemoveAt(1)
        .FluentReverse()
        .FluentForEach(value => value.WriteLine())
        .FluentClear();
}
```

**[⬆ back to top](#table-of-contents)**

</details>

<details>
  <summary><b>Prefer composition over inheritance</b></summary>

As stated famously in [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition.

The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a" relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class (Change the caloric expenditure of all animals when they move).

**Bad:**

```csharp
class Employee
{
    private string Name { get; set; }
    private string Email { get; set; }

    public Employee(string name, string email)
    {
        Name = name;
        Email = email;
    }

    // ...
}

// Bad because Employees "have" tax data.
// EmployeeTaxData is not a type of Employee

class EmployeeTaxData : Employee
{
    private string Name { get; }
    private string Email { get; }

    public EmployeeTaxData(string name, string email, string ssn, string salary)
    {
         // ...
    }

    // ...
}
```

**Good:**

```csharp
class EmployeeTaxData
{
    public string Ssn { get; }
    public string Salary { get; }

    public EmployeeTaxData(string ssn, string salary)
    {
        Ssn = ssn;
        Salary = salary;
    }

    // ...
}

class Employee
{
    public string Name { get; }
    public string Email { get; }
    public EmployeeTaxData TaxData { get; }

    public Employee(string name, string email)
    {
        Name = name;
        Email = email;
    }

    public void SetTax(string ssn, double salary)
    {
        TaxData = new EmployeeTaxData(ssn, salary);
    }

    // ...
}
```

**[⬆ back to top](#table-of-contents)**

</details>






