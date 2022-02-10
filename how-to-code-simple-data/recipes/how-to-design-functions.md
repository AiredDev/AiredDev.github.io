The HtDF recipe consists of 7 steps:
1. Signature
	* Declares the type of data the function consumes and produces
	* Primitive types are Number, Integer, Natural, String, Image, Boolean
	* `Type ... -> Type`
2. Purpose
	* One-line description of what the function produces in terms of what it consumes
	* Says more than the signature
3. Stub
	* Sort of like scaffolding. We comment it out or delete it later on
	* A function definition that has the correct function name, the correct number of parameters, and produces a dummy result of the correct type
	* Ensures that the examples and tests written in the following step actually run (even with incorrect results)
4. Examples and tests
	* Sometimes it's easier to produce a general function if we start with very specific examples of what it needs to do
	* Use multiple examples to illustrate behaviour
5. Template
	* Outlines the function
	* There are several templates. For now, just use `(... x)` where `x` is the argument to the function.
6. Code the body
	* Use everything written before to figure out what to do. It sometimes helps to elaborate on examples to show how the expected value could have been produced.
7. Test and debug