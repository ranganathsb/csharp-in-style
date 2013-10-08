### using Statements

Group using statements by common prefix, with shorter namespaces coming before longer ones, creating neat clusters of statements separated by single empty lines.

Namespaces should be ordered from platform-neutral through platform-specific, with .NET namespaces first, then library or component namespaces, then Xamarin namespaces, then application namespaces:

```csharp
// Beautiful:
using System;
using System.Linq;
using System.Collections.Generic;

using MyLib;
using MyLib.Extensions;

using MonoTouch.UIKit;
using MonoTouch.Foundation;

using MyApp;

// Disaster:
using MyLib.Extensions;
using MonoTouch.Foundation;
using System.Collections.Generic;
using System;
using System.Linq;
using MonoTouch.UIKit;
using MyLib;
```

Prune redundant namespaces aggressively.

### Declaring Types

```csharp
class MyClass : BaseClass, IDoesThis {
}
```

* Put a space before and after `:`.

#### Enums

Simple enums may be defined on a single line:

```csharp
enum Edge { Left, Right, Bottom, Top }
```

Larger enums should list entries on separate lines and always end in a comma:

```csharp
public enum StringSplitOptions {
	None                = 0,
	RemoveEmptyEntries  = 1,
}
```

### Type Inference

Use it. Less typing is almost always better than more typing, with some important exceptions.

Use `var` when the type is repeated on the right-hand side of the assignment:

```csharp
// Perfect!
var users = new Dictionary<UserId, User> ();

// Bloated.
Dictionary<UserId, User> users = new Dictionary<UserId, User> ();
```

Don't use `var` for capturing the return type of a method or property when the type is not apparent:

```csharp
// Horrendous.
var things = Interpret (data);

// Much better.
HashMap<Thing> things = Interpret (data);
```

Omit the type when using array initializers:

```csharp
// Could be better:
database.UpdateUserIds (new int[] { 1, 2, 3 });

// Better:
database.UpdateUserIds (new [] { 1, 2, 3 });
```

### Object and Collection Initializers

Prefer object and collection initializers.

When using object and collection initializers, every expression should be on a separate line, and every line should end with a comma `,`. Try to line up property assignments in object initializers. ( **Rationale**: When new values are added to the end, there won't be "noise" to add a comma to the previous enum member.)

```csharp
var entries = new Dictionary<string, int> () {
	{ "key1", 1 },
	{ "key2", 2 },  // All entries end with ','
};

var lsCommand = new ProcessStartInfo ("ls", "/") {
	RedirectStandardInput   = true,
	RedirectStandardOutput  = true,
	RedirectStandardError   = true,
	UseShellExecute         = false,
};
```

### Lambdas

Lambdas are written with a single space before and after the `=>`:

```csharp
// Great.
Func<int, int> square = i => i * i;

// Terrible.
Func<int, int> square = i=>i * i;
```

Always prefer lambdas, `Func<>`, and `Action<>` types to `delegate`. The only recommended use of `delegate` is when the body of your anonymous method doesn't reference any of its arguments:

```csharp
thing.EventWithSenderAndEventArgs += delegate {
	Console.WriteLine ("EventWithSenderAndEventArgs raised.");
};
```

If your lambda takes a single argument, omit the parentheses around the argument list:

```csharp
// Great!
var admins = Users.Select (user => user.IsAdministrator);

// Silly.
var admins = Users.Select ((user) => user.IsAdministrator);
```

Whenever possible, omit types from lambda argument lists, and use simple names:

```csharp
// Great:
list.OnScroll += (sender, e) => {
	...
};

// Passé:
list.OnScroll += (object sender, EventArgs e) => {
	...
};
```

When the body of a lambda is a simple statement or expression, don't use a block:

```csharp
// Excellent!
var averageSalary = employees.Average (employee => employee.Salary);

// Inconceivable!
var averageSalary = employees.Average (employee => { return employee.Salary; });
```

It is acceptable to use single-character argument names in lambdas if the receiver is an `IEnumerable` and is named in such a way as to make the lambda argument obvious, and the lambda argument name is the first character of the receiver's identifier:

```csharp
// Acceptable:
var averageSalary = employees.Average (e => e.Salary);

// Acceptable:
var averageSalary = employees.Average (employee => employee.Salary);
```

### Indentation

`switch` statements have the case at the same indentation as the `switch`:

```csharp
switch (x) {
case 'a':
	...
case 'b':
	...
}
```

### Performance and Readability

It is more important to be correct than to be fast.

It is more important to be maintainable than to be fast.

Fast code that is difficult to maintain is likely going to be looked down upon.

### Where to put spaces

Use a space before an opening parenthesis when calling functions, or indexing, like this:
```csharp
method (a);
b [10];
```

Do not put a space after the opening parenthesis and the closing one, ie:

good:

```csharp
method (a);	
array [10];
```

bad:

```csharp
method ( a );	
array[ 10 ];
```

Do not put a space between the generic types, ie:

good:

```csharp
var list = new List<int> ();
```

bad:

```csharp
var list = new List <int> ();
```

Separate type parameters to generic types by a space:

```csharp
// Excellent.
var users = new Dictionary<UserId, User> ();

// Worthless.
var users = new Dictionary<UserId,User> ();
```

### Where to put braces

Inside a code block, put the opening brace on the same line as the statement:

good:

```csharp
if (a) {
	code ();
	code ();
}
```

bad:

```csharp
if (a) 
{
	code ();
	code ();
}
```

Omitting braces for single line if statements is acceptable, however braces are always acceptable:

acceptable:

```csharp
if (a)
	code ();
```

good:

```csharp
if (a) {
	code ();
}
```

Unless there are either multiple hierarchical conditions being used or that the condition cannot fit into a single line.

good:﻿

```csharp
if (a) {
	if (b) {
		code ();
	}
}
```

acceptable:﻿

```csharp
if (a) {
	if (b)
		code ();
}
```

bad:

```csharp
if (a)
	if (b)
		code ();
```

When defining a method, use the C style for brace placement, that means, use a new line for the brace, like this:

good:

```csharp
void Method ()
{
}
```

bad:

```csharp
void Method () {
}
```

Properties and indexers are an exception, keep the brace on the same line as the property declaration.

Rationale: this makes it visually simple to distinguish them.

good:

```csharp
int Property {
	get {
		return value;
	}
}
```

bad:

```csharp
int Property 
{
	get {
		return value;
	}
}
```

Notice how the accessor "get" also keeps its brace on the same line.

For very small properties, you can compress things:

ok:

```csharp
int Property {
	get { return value; }
	set { x = value; }
}
```

Empty methods: They should have the body of code using two lines, in consistency with the rest:

good:

```csharp
void EmptyMethod ()
{
}
```

bad:

```csharp
void EmptyMethod () {}
void EmptyMethod () {
}
void EmptyMethod () 
{}
```

Generic method type parameter constraints are one separate lines, one line per type parameter, before the opening brace:

```csharp
static bool TryParse<TEnum>(string value, out TEnum result)
	where TEnum : struct
{
	...
}
```

If statements with else clauses are formatted like this:

good:

```csharp
if (dingus) {
        ...
} else {
        ... 
}
```

bad:

```csharp
if (dingus) 
{
        ...
} 
else 
{
        ... 
}
```

bad:

```csharp
if (dingus) {
        ...
} 
else {
        ... 
}
```

Classes and namespaces go like if statements, differently than methods:

good:

```csharp
namespace N {
	class X {
		...
	}
}
```

bad:

```csharp
namespace N
{
	class X
	{
		...
	}
}
```

As an exception, when the type is _both_ generic _and_ the type parameters have constraints, the opening brace should go on a newline, and each constraint gets a separate line:

```csharp
namespace N {

	class WeakReference<T>
		where T : class
	{
		...
	}
}
```

So, to summarize:

| Statement	                 | Brace position |
|--------------------------------|----------------|
| Namespace                      | same line |
| Type                           | same line for types without generic parameter constraints |
| Method (including ctor)        | new line |
| Properties                     | same line |
| Control blocks (if, for...)    | same line |
| Anonymous types and methods    | same line |

### Multiline Parameters

When you need to write down parameters in multiple lines, indent the parameters to be below the previous line parameters and indented two tab stops, like this:

Good:

```csharp
WriteLine (format, foo,
		bar, baz);
```

If you do not want to have parameters in the same line as the method invocation because you ar running out of space, you can indent the parameters in the next line, like this:

Good:

```csharp
WriteLine (
		format, moved, too, long);
```

Comma separators go at the end, like a good book, never at the beginning:

Good:

```csharp
WriteLine (foo,
		bar,
		baz);
```

Atrocious:

```csharp
WriteLine (foo
		, bar
		, baz);
```

Use whitespace for clarity

Use white space in expressions liberally, except in the presence of parenthesis:

good:

```csharp
if (a + 5 > method (blah () + 4))
```

bad:

```csharp
if (a+5>method(blah()+4))
```

### File headers

For any new files, please use a descriptive introduction, like this:

```csharp
//
// System.Comment.cs: Handles comments in System files.
//
// Author:
//   Juan Perez (juan@address.com)
//
// Copyright (C) 2002 Address, Inc (http://www.address.com)
//
```

If you are modyfing someone else's code, and your contribution is significant, please add yourself to the Authors list.

### Comments

Comments begin with `// ` (note the single space), and use sentence casing, grammar, and proper spelling.

```csharp
// Great:
// Verify that the client and server states are consistent.

// Bad - missing space:
//Verify that the client and server states are consistent.


// Bad - not a sentence:
// verify client server states
```

If your comment just paraphrases code, remove it:

```csharp
// Bad
// Makes the window key and orders it front.
window.MakeKeyAndOrderFront ();
```

### Multiline comments

For long, multiline comments use the following style:

```csharp
/*
 * Blah
 * Blah again
 * and another Blah
 */
```

### Casing

Argument names should use the camel casing for identifiers, like this:

good:

```csharp
void Method (string myArgument)
```

bad:

```csharp
void Method (string lpstrArgument)
void Method (string my_string)
```

Instance fields should use underline as a separator:

good:

```csharp
class X {
	string my_string;
	int    very_important_value;
}
```

passable:

```csharp
class X {
      string myString;
      int    veryImportantValue;
}
```

bad:

```csharp
class X {
	int      _intval;
	string   m_string;
}
```

The use of "m_" and "_" as prefixes for instance members is highly discouraged.

An exception to this rule is serializable classes. In this case, if we desire to have our serialized data be compatible with Microsoft's, we must use the same field name.

### `this`

The use of "this." as a prefix in code is discouraged, it is mostly redundant. In general, since internal variables are lowercase and anything that becomes public starts with an uppercase letter, there is no ambiguity between what the "Foo" and "foo" are. The first is a public property or field, the second is internal property or field.

Good:

```csharp
class Foo {
	int bar;
 
	void Update (int newValue)
	{
		bar = newValue;
	}
 
	void Clear ()
	{
		Update ();
	}
}
```

Bad:

```csharp
class Foo {
	int bar;
 
	void Update (int newValue)
	{
		this.bar = newValue;
	}
 
	void Clear ()
	{
		this.Update ();
	}
}
```

An exception is made for "this" when the parameter name is the same as an instance variable, this happens sometimes in constructors or if naming is difficult:

Good:

```csharp
class Message {
	char text;
 
	public Message (string text)
	{
		this.text = text;
	}
}
```

### Line length and alignment

Line length: The line length for C# source code is 80 columns.

If your function declaration arguments go beyond this point, please place them on new lines, indenting by two tab stops (preferred), or align with the opening brace (acceptable). ( **Rationale** : By not aligning to the opening brace, you can refactor/rename the method in the future without "screwing up" the indentation of all remaining parameters.)

When aligning to the opening brace, use the same number of tabs used on the first line followed by enough spaces to align the arguments. This ensures that the arguments will remain aligned when viewed with a different tabsize. In the following example, the line that declares argc is indented with 2 tabs and 14 spaces:

```csharp
namespace N {
	class X {
		// Preferred
		void Method1 (int arg, string argb,
				int argc)
		{
		}

		// acceptable
		void Method2 (int arg, string argb,
		              int argc)
		{
		}
	}
}
```

When a _condition_ for a branch or loop construct requires multiple lines, each line should be indented by two tab stops. ( **Rationale** : it keeps the condition from looking like it's part of the body.) Complicated and nested expressions should use spaces to align with the originating expression. Boolean operators should be at the end of the line.

```csharp
void M ()
{
	if (string.IsNullOrEmpty (DataHost) &&
			string.IsNullOrEmpty (DataPort) &&
			string.IsNullOrEmpty (DataScheme))
		DoSomething ();
	if (!method.IsConstructor &&
			method.Name == "SomeMethod" &&
			(method.IsVirtual ||
			 method.IsAbstract))
		DoSomethingElse ();
}
```

When invoking functions, the rule is different, the arguments are not aligned with the previous argument, instead they begin at two tab stops, like this. When "chaining" method calls, each "chain" should be on a new line (within reason) and each sub-expression should be indented one tab stop.

```csharp
void M ()
{
	MethodCall ("Very long string that will force",
			"Next argument on the 8-tab pos",
			"Just like this one");
	IEnumerable<int> items = Enumerable.Range (0, 100)
		.Where (e => (e % 2) == 0)
		.Select (e => e*2);
}
```

_Consider_ lining up variable names and assignment expressions, including in class fields. When doing so, use spaces -- _not_ tabs -- to align identifiers. (Once a non-tab character has been used on a line, tab should not be used again. `git diff` will hilight such lines in "red.")

```csharp
class C {

	string          Name    = "name";
	List<string>    Values  = new List<string> ();

	void M ()
	{
		var url               = new Uri ("http://www.example.com");
		var request           = (HttpWebRequest) WebRequest.Create (url);
		request.Method        = "GET";
		WebResponse response  = request.GetResponse ();
		...
	}
}
```

### Baroque Coding

Baroque coding is discouraged.

We discourage the use of the "private" keyword to flag internal fields or methods since this is the default visibility mode in C#. The keyword exists because of Java. Avoid it, it merely is more line noise for people that are reading your code.

But the same principle applies everywhere else in Mono. Avoid complex code or redundant code for the sake of it. Try to write the minimum amount of text possible.
