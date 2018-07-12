### Majoris Language Specification 1.0

#### Description:
	A transformation language.
			
#### Influenced by:
	C, Lua and GLSL.
			
#### Language semantic:
	Each line instructions must be separed by semicolons (";") with the exception of language lexical structures that ends enclosed by brackets or parenthesis.
	
###### Arithmetic operators:
	+: addition or string concatenation.
	-: subtraction
	*: multiplication
	/: division
	^: power
	%: modulo
	
###### Relational operators:
	> : greater than
	>=: greater or equal than
	< : smaller than
	<=: smaller or equal than
	==: equal
	!=: not equal
	
###### Logical operators:
	! : NOT
	&&: AND
	||: OR
	? : select AND
	: : select OR
		
###### Bitwise operators:
	~ : bitwise NOT
	& : bitwise AND
	| : bitwise OR
	^ : bitwise XOR
	<<: bitwise left shift
	>>: bitwise right shift

#### Variable names:
	UTF-8 for variable names must be supported. The language however reserves its right for exclusive usage of any keyword, syntax or operator. These can not be declared as variable name (except for foreign compiled name objects).
			
#### Variable shadowing:
	Variable shadowing is not allowed and compiler will error with any variable declaration masking another. Structs expands are allowed to replace any other declaration or initialization inherited.

#### Data types implementations:
###### void:
	Stores nothing. The only value that can be assigned explicitly is "nil".
	Operations is already set on dynamic or typed languages.
	Must be declared as ```cvoid Varname;```
		
###### number:
	Stores real numbers.
	Operations is already set in dynamic languages. For typed languages the compiler will default to "auto".
	Must be declared as ```cnumber Varname;```
	
###### string:
	Stores sequence of characters. A length value must be specified in their declaration.
	Operations is already set in dynamic languages. For typed languages the compiler will generate its own library with type "uint8_t" or use language default implementation.
	The language reserves its right for automatic string conversions when required by a function argument or any other expression when possible.
	Must be declared as ```cstring Varname[Length];```
		
###### bool:
	Store boolean values ("true" or "false").
	Operations is already set in dynamic languages. For typed languages the compiler will default to uint8_t or use language default implementation.
	Must be declared as ```cbool Varname;```
		
###### vec2:
		Two dimension vector ("x" or "r", "y" or "g"). Stores two real numbers each. Swizzling indices is supported and a index can be accessed as ".x". Combining 2 indices at same time (as ".xy") will return a vec2. 
		Will compile to raw and inlined numbers operations or SIMD. For typed languages the compiler will default operation variables to "auto".
		Must be declared as ```cvec2 Varname;```
		
###### vec3:
		Three dimension vector ("x" or "r", "y" or "g", "z" or "b"). Stores three real numbers each. Swizzling indices is supported and a index can be accessed as ".x". Combining 2 or 3 (as ".xy" or ".xyz") indices at same time will return a vec2 or vec3 respectively.
		Will compile to raw and inlined numbers operations or SIMD. For typed languages the compiler will default operation variables to "auto".
		Must be declared as ```cvec3 Varname;```
		
###### vec4:
		Three dimension vector ("x" or "r", "y" or "g", "z" or "b", "w" or "a"). Stores four real numbers each. Swizzling indices is supported and a index can be accessed as ".x". Combining 2, 3 or 4 indices at same time will return a vec2, vec3 or vec4 respectively.
		Will compile to raw and inlined numbers operations or SIMD. For typed languages the compiler will default operation variables to "auto".
		Must be declared as ```cvec4 Varname;```
		
###### matNxN (like "mat22", "mat33", "mat44", etc.):
		NxN dimension matrix. Each row or column can be swizzled to vec2, vec3 or vec4.
		Will compile to raw and inlined numbers operations or SIMD. For typed languages the compiler will default operation variables to "auto".
		Must be declared as ```cmatNN Varname;```
		
#### Data structures:
###### struct:
		A custom data structure can be declared based on standard types.
		```c
		struct Name
		{
			<datatype> Varname
			<datatype> Varname[ArrayLength]
		}
		```
###### array:
		Arrays can be declared based on standard types and declared structs. Array can be declared inside struct declarations.
		```c<datatype> Varname[ArrayLength];```
		
#### Data declaration initialization syntax:
###### void:
		```c
		void Varname;
		void Varname = nil;
		```
		
###### number:
		```c
		number Varname;
		number Varname = 5;
		number Varname = 0;
		number Varname = -1.0;
		number Varname = 0x03;
		number Varname = 0.2f;
		```
		
###### string:
		```c
		string Varname[5]
		string Varname[] = "Hello";
		string Varname[5] = "Hello";
		```
		
######## Note 1: Empty string length field is ONLY allowed when the string is declared explicitly. The value assigned can not be a concatenation operation nor function call.
######## Note 2: String initialization fields can be smaller or equal to declared string size when specified. Initializing bigger values than the specified is not allowed.
		
######## bool:
	```c
	bool VarnaForeignme = false;
	bool Varname = true;
	```
	
######## vec2:
	```c
	vec2 Varname = vec2(2.0, -1);
	
######## vec3:
	```c
	vec3 Varname = vec3(2.0, -1, 0.5);
	
######## vec4:
	```c
	vec4 Varname = vec4(2.0, -0.2, 0.15, -6.0);
	
######## matNN:
	```c
	matNN Varname = matNN(vecN(...), ...));
	
######## struct:
	```c
	struct Example
	{
		number Price;
		string Name[12];
	}

	<struct name> <variable name> = {.<struct data index> = <value>, ...};
	Example Test[2] = {{.Price = 1; .Name = "test"}, {.Price = 0.59; .Name = "test2"}};
	```
	
######## array:
	```c
	datatype Varname[] = {ValueIndex1, ...}
	datatype Varname[ArrayLength] = {ValueIndex1, ...}
	```
	
#### Conditionals declaration:
	```c
	if(<logical expression>)
	{
		<code>
	}
	elseif(<logical expression>)
	{
		<code>
	}
	else
	{
		<code>
	}
	```
	
#### Loops declaration:
###### for:
	```c
	for(<initial number value>, <target number value>, <step number value>)
	{
		<code>
	}
	```
	
###### while:
	```c
	while(<logical expression>)
	{
		<code>
	}
	```
	
###### repeat:
	```c
	repeat
	{
		<code>
	}
	until(<logical expression>)
	```
	
#### Function declaration:
	```c
	<datatype> Name(<datatype> Arg1, ...)
	{
		<code>
		return;
	}
	```
	
###### Note 1: Functions can have a maximum of 127 arguments or compiler will error.
###### Note 2: Functions can be overloaded if declared twice, but must return the same datatype.

#### Function declaration inside structs and operator overload:
	```c
	struct Example
	{
		number Price = 0.0;
	
		number getPrice()
		{
			return Price;
	    }
	
		void setPrice(number Price2)
		{
			Price = Price2;
		}
	
		Example +(Example Arg1, Example Arg2)
		{
			Example ExchangeVar = new Example();
			ExchangeVar:setPrice(Arg1.Price + Arg2.Price);
			return ExchangeVar;
		}
		
		void _init(number Arg1)
		{
			Price = Arg1;
		}
	}
	```
	
###### Note 1: Operators overload can be declared with the operator as function name. The above example shows overload for operator "+".
###### Note 2: Memory management for objects created inside overload will be managed by compiler.
###### Note 3: Functions declared inside structs can not be called directly by other functions declared the same way. An exchanging object like in the example above for overload function is necessary.
###### Note 4: _init function is only called when combined with "new" keyword and its declaration is not required.
	
#### Recursive function declarations:
###### The following function will generate an infinite loop as an example:
	```c
	void Name()
	{
		Name();
	}
	```
	
#### Struct inheritance declaration:
###### Keyword "expand" must be used with a different struct name, values or functions with same name inside will replace the original one.
	```c
	expand Example struct ExampleTwo
	{
		number Price = 2.0;
	}
	```
	
#### Struct private keyword:
###### Keyword "private" can be used as modifier for variables usage in its internal functions. It can not be used together with "foreign" keyword. Example:
	```c
	struct PrivateExample
	{
		private number Test;
	
		void doStuff()
		{
			Test = 1;
		}
		
		void doStuff2()
		{
			Test = 2;
		}
	}
	```
#### Foreign objects interface:
	The "foreign" keyword is used to identify data structures that come outside of code. They are avaiable in the compiled language's framework. To work with them they must be declared before as below:
	
###### Foreign function declaration:
	```c
	foreign (<compiled name>) <datatype> <Name>(<datatype> Arg1, ...);
	foreign (love.graphics.setColor) void love_graphics_setColor(number r, number g, number b, number a);
	```
	
###### Complex foreign objects declaration example abstracting love's ByteData API:
	```c
	foreign struct Pointer{}

	foreign struct Data
	{
		Data clone();
		Pointer getPointer();
		number getSize();
		string[] getString();
		bool release();
		string[] type();
		bool typeOf(string[] Name);
	}

	foreign (print) void print(string Var);

	foreign (love.data.newByteData) void love_data_newByteData(string[] Data);
	```
	
#### Comments and comment blocks:
	```c
	// for line comments
	/* comment */ for comment blocks
	```
	
#### Language reserved keywords:
	foreign, private, struct, expand, if, elseif, else, return, break, repeat, until, while and nil

#### Compiler notes:

###### Optimizations:
	Compiler is allowed to optimize expressions, inline functions, precompute loops and remove dead code although optimization implementation is not required, but if done must have the ability to disable optimization.

###### Obfuscation:
	Compiler is allowed to obfuscate output and also generate unreadable pieces of code for proper syntax conversion. For explicit obfuscation the compiler must have the ability to enable obfuscation.
