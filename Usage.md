# Introduction #

PhpWsdl can produce WSDL of course. But did you know that PhpWsdl can also run
a SOAP server for you?

# Details #

PhpWsdl was designed to produce WSDL from PHP. The WSDL definitions can be
defined in comment blocks or from PHP code. The final WSDL can be used with
the native PHP SoapServer.

## Why NuSOAP or Zend? ##

Adapters for NuSOAP or Zend can run the NuSOAP or Zend SOAP server. But f.e.
the NuSOAP SOAP server can't handle the WSDL produced by PhpWsdl. If you use
this adapter, PhpWsdl will only produce the HTML documentation and make type
and method registrations to the soap\_server object for you. The WSDL needs
then to be created by the NuSOAP framework.

So why would you use the NuSOAP adapter f.e.? I found creating a SOAP
webservice using the NuSOAP framework was too complex for my purposes. I
didn't want to write too much code that would only work with NuSOAP - I want
to keep my applications mostly independent. But I already run SOAP webservices
using NuSOAP. To change to PHPs native SOAP support I'd have to change a lot
of old code and to write a lot of new code. But PhpWsdl makes it easy to
support both: NuSOAP and PHP SoapServer. Since I always use comment blocks to
comment my code, PhpWsdl was able to serve my webservices from scratch. Now I
have two webservices: One with PhpWsdl and the NuSOAP adapter and another
endpoint with PhpWsdl and native PHP SoapServer. Any changes to my webservices
are now served with both WSDL definitions (from NuSOAP and from PhpWsdl), so
my existing customers don't need to rewrite their client applications.

The same is for the webservices I've created using the Zend SOAP server.

Of course I recommend changing to the new endpoint for existing customers and
consuming only the new endpoint, because at the right time I want to get rid
of Zend and NuSOAP.

## Basic usage ##

If you develop your PHP application with Eclipse f.e. it's very easy to
produce comment blocks since Eclipse will create them for you if you just type
"/" and hit enter. PhpWsdl defined a rule: One WSDL type or method
definition per comment block. AND: All methods must be within one webservice
handler class! OR: You mix class methods and global methods.

There is an option to use global methods (not using a handler class). You may
also mix class and global methods (what is not possible in PHP SoapServer per
default!).

### WSDL definitions in comment blocks ###

#### Service ####

PhpWsdl is able to determine the webservice name when using a handler class.
But if you have no handler class, there are two ways to tell PhpWsdl the
webservice name:

1. Using the PhpWsdl->Name property
You can give a name in the constructor or you set the property in your code.
If you're using a handler class, PhpWsdl can determine the name.

2. Using a comment block and the @service keyword
Since version 2.2 you can set the webservice name with the @service keyword
within a comment block:

```
/**
 * Documentation, may be multiline
 * 
 * @service ServiceName
 */
```

Then "ServiceName" would be the name of your service.

If you only have global methods in your webservice, you don't need to define a
webservice name. PhpWsdl will then use "SoapWebService" per default.

**Note**: If you use a handler class, the name of the service has to match the
name of this class!

#### Methods ####

This is an example for a comment block of a method:

```
/**
 * Say hello demo
 * 
 * @param string $name Some name (or an empty string)
 * @return string Response string
 */
public function SayHello($name){
	$name=utf8_decode($name);// Because a string parameter is UTF-8 encoded...
	if($name=='')
		$name='unknown';
	return utf8_encode('Hello '.$name.'!');// Because the return value is expected to be UTF-8 encoded
}
```

If the method has no parameters, the @param isn't required. The same for the
return value. This will only work for public methods.

The basic syntax for a comment block is:

```
/**
 * Documentation, may be multiline
 * 
 * @param [type] [parameter] Some single line documentation
 * @return [type] Some single line documentation
 */
```

[type](type.md) may be any type that is predefined by the W3C (and included in the list
of basic types in PhpWsdl::$BasicTypes) or defined as complex type within your
application. [parameter](parameter.md) is always the parameter name of the method.

To hide a public method from WSDL, simply use the keyword @ignore to jump over
the next method:

```
/**
 * @ignore Next method should be hidden even if it's public!
 */
```

##### Global methods #####

Because the PhpWsdlParser don't know if a method is within or outside  of a
class, you have to tell the parser how to handle a method. This can be done
with a setting:

```
/**
 * Say hello demo
 * 
 * @param string $name Some name (or an empty string)
 * @return string Response string
 * @pw_set global=1 <- Declare this method as global (=not within a class)
 */
function SayHello($name){
	$name=utf8_decode($name);// Because a string parameter is UTF-8 encoded...
	if($name=='')
		$name='unknown';
	return utf8_encode('Hello '.$name.'!');// Because the return value is expected to be UTF-8 encoded
}
```

By default methods are declared as class methods. If you want PhpWsdl to add
all methods as global methods per default, change the
PhpWsdlMethod::$IsGlobalDefault property to TRUE, before PhpWsdl runs:

```
PhpWsdlMethod::$IsGlobalDefault=true;
```

Then you don't need "@pw\_set global=1" for your global methods anymore. But if
you want to mix class and global methods, you need to declare class methods as
non-globals:

```
class SoapDemo{
	/**
	 * Say hello demo
	 * 
	 * @param string $name Some name (or an empty string)
	 * @return string Response string
	 * @pw_set global=0 <- Declare this method as non-global (=within a class)
	 */
	function SayHello($name){
		$name=utf8_decode($name);// Because a string parameter is UTF-8 encoded...
		if($name=='')
			$name='unknown';
		return utf8_encode('Hello '.$name.'!');// Because the return value is expected to be UTF-8 encoded
	}
}
```

#### Complex types ####

To support complex type definitions from comment blocks, PhpWsdl defined new
keywords:

  * @pw\_element defines an element of a complex type
  * @pw\_complex creates the complex type

The comment block syntax is for example:

```
/**
 * This is how to define a complex type f.e. - the class ComplexTypeDemo doesn't need to exists, 
 * but it would make it easier for you to return that complex type from a method
 *
 * @pw_element string $StringA A string with a value
 * @pw_element string $StringB A string with a NULL value
 * @pw_set nillable=false Not NULL
 * @pw_element int $Integer An integer
 * @pw_set nillable=false Not NULL
 * @pw_element boolean $Boolean A boolean
 * @pw_complex ComplexTypeDemo The complex type name definition
 */
class ComplexTypeDemo{
	public $StringA='String A';
	public $StringB=null;
	public $Integer=123;
	public $Boolean=true;
}
```

This will define a complex type that matches the ComplexTypeDemo so your
method can return a ComplexTypeDemo object.

The basic syntax is:

```
/**
 * Documentation, may be multiline
 *
 * @pw_element [type] [name] Some single line documentation
 * @pw_complex [name] Some single line documentation
 */
```

#### Arrays ####

Arrays can be defined from any type. Example:

```
/**
 * @pw_complex stringArray A string array type
 */
/**
 * @pw_complex ComplexTypeDemoArray An array of ComplexTypeDemo
 */
```

The basic syntax is:

```
/**
 * Documentation, may be multiline
 * 
 * @pw_complex [type]Array Some single line documentation
 */
```

The only thing you have to do is to append the postfix "Array" to the original
type name.

Another way to define an array without a name restriction is to add [.md](.md) to its
name. The [.md](.md) is only required for parsing:

```
/**
 * @pw_complex arrayOfInt[] int An int array type
 */
```

You can find and use this example array type with the name "arrayOfInt".

The basic syntax is:

```
/**
 * Documentation, may be multiline
 * 
 * @pw_complex [name][] [type] Some single line documentation
 */
```

### Create an instance of PhpWsdl ###

Full example:

```
require_once('class.phpwsdl.php');
$soap=PhpWsdl::CreateInstance(
	null,	// Set this to your namespace or let PhpWsdl find one
	null,	// Set this to your SOAP endpoint or let PhpWsdl determine it
	null,	// Set this to a writeable folder to enable caching
	null,	// Set this to the filename or an array of filenames of your 
			// webservice handler class(es) (be sure to add the file that 
			// contains the handler class as first class definition at 
			// first)
	null,	// Set this to the webservice handler class name or let 
			// PhpWsdl determine it
	null,	// If you want to define some methods from code, give an array 
			// of PhpWsdlMethod here
	null,	// If you want to define some types from code, give an array of 
			// PhpWsdlComplex here
	false,	// Set this to TRUE to output WSDL on request and exit after 
			// WSDL has been sent
	false	// Set this to TRUE to run the SOAP server and exit
);
```

In this example all constructor parameters have their default values, so you
could also use this code that has the same effect:

```
require_once('class.phpwsdl.php');
$soap=PhpWsdl::CreateInstance();
```

### Create WSDL ###

```
$wsdl=$soap->CreateWsdl();
```

### Create HTML ###

```
$html=$soap->OutputHtml(false,false);
```

### Create PHP ###

```
$php=$soap->OutputPhp(false,false);
```

### Run the SOAP server ###

```
$soap->RunServer();
```

### The quick mode ###

The quick mode will handle all this with a single line of code:

  * create HTML documentation
  * create a PHP SOAP client proxy
  * output WSDL on request
  * run a SOAP server

To enable the quick mode:

```
require_once('class.phpwsdl.php');
PhpWsdl::RunQuickMode();
```

This requires the WSDL definitions and the webservice handler class to be in
the same file. Or in the file "class.webservice.php" in the same folder.

To define the webservice handler class file:

```
require_once('class.phpwsdl.php');
PhpWsdl::RunQuickMode('class.handlerclass.php');
```

Or to define multiple files:

```
require_once('class.phpwsdl.php');
PhpWsdl::RunQuickMode(Array('class.soapdemo.php','class.complextypedemo.php'));
```

### Autorun ###

The quick mode can be combined with autorun, so PhpWsdl will run and exit
after the class has been loaded:

```
$PhpWsdlAutoRun=true;
require_once('class.phpwsdl.php');
```

The autorun can be done by setting the global variable $PhpWsdlAutoRun to TRUE
or by editing the source of class.phpwsdl.php and setting the static property
PhpWsdl::$AutoRun to TRUE.

The autorun will use the quick mode. Any code after the "require\_once" won't
be executed.

## Advanced usage and full examples ##

PhpWsdl has many static properties and many properties that are only available
in an instance. The same for methods. The source of PhpWsdl is fully
documented, so you may get the usage documentation from IntelliSense of your
PHP development IDE or from the source code.

Some advanced usage examples are also included in the `demo*.php` files that
are provided within the PhpWsdl downloads. The downloads also includes
demonstrations for the basic usages.

## Plugins and extensions ##

PhpWsdl supports developing plugins and extensions. This is not documented
yet, but if you need to make a plugin or extension, please use the hooking.
Contact me if you need an additional hook for your plugin. Of course I'm
interested in what you do with plugins and extensions - maybe you want to
distribute them here within this project on Google Code, too?

There are already some plugins and extensions available for download in this
Google Code project in the Downloads section. If there is a new version of
PhpWsdl, I try to make all changes compatible to the existing versions of the
plugins and extensions. Sometimes this isn't possible. Then I will also
provide a new version of a plugin or extension for the new version of PhpWsdl
that matches the current PhpWsdl version number in the download filename.