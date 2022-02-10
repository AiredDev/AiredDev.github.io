The HtDF recipe consists of 7 steps:
1. Signature
	a. Declares the type of data the function consumes and produces
	b. Primitive types are Number, Integer, Natural, String, Image, Boolean
	c. `Type ... -> Type`
2. Purpose
	a. One-line description of what the function produces in terms of what it consumes
	b. Says more than the signature
3. Stub
	a. Sort of like scaffolding. We comment it out or delete it later on
	b. A function definition that has the correct function name, the correct number of parameters, and produces a dummy result of the correct type
	c. Ensures that the examples and tests written in the following step actually run (even with incorrect results)
4. Examples and tests
	a. Sometimes it's easier to produce a general function if we start with very specific examples of what it needs to do
	b. Use multiple examples to illustrate behaviour
5. Template
	a. Outlines the function
	b. There are several templates. For now, just use `(... x)` where `x` is the argument to the function.
6. Code the body
	a. Use everything written before to figure out what to do. It sometimes helps to elaborate on examples to show how the expected value could have been produced.
7. Test and debug