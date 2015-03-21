# Introduction #

A WSDL complex type.

# Details #

Currently PhpWsdl supports complex types with sequenced elements of mixed types. PhpWsdl also supports simple arrays.

To create a complex type:

```
/**
 * The description
 * 
 * @pw_element string $name An element with type "string"
 * @pw_element string $email Another element with type "string"
 * @pw_complex SampleComplex End the definition+
 */
```

To create an array simply add "Array" to the type name:

```
/**
 * The description
 * 
 * @pw_complex stringArray An array of string
 */
/**
 * The description
 * 
 * @pw_complex arrayOfInt[] int An array of int
 */
/**
 * The description
 * 
 * @pw_complex SampleComplexArray An array of SampleComplex
 */
```