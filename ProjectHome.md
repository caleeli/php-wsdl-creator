

# PhpWsdl #

I started to develop my own WSDL generator for PHP because the ones I saw
had too many disadvantages for my purposes. The main problem - and the main
reason to make my own WSDL generator - was receiving NULL in parameters
that leads the PHP SoapServer to throw around with "Missing parameter"
exceptions. F.e. a C# client won't send the parameter, if its value is
NULL. But the PHP SoapServer needs the parameter tag with 'xsi:nil="true"' to
call a method with the correct number of parameters. I found a solution for
this problem and developed a complex type supporting WSDL generator that can
also run the PHP SoapServer with only a single line of code. I hope my
solution is helpful for you, too.

## Example usage ##

**The fastest usage (ever? ;):**

```
require_once ( 'class.phpwsdl.php' );
PhpWsdl::RunQuickMode ( );
```

This will run the PHP SoapServer and determine all the configuration, if your webservice handler class is within the same file or in a file named 'class.webservice.php'.

**If the webservice handler _class is in another file_:**

```
require_once ( 'class.phpwsdl.php' );
PhpWsdl::RunQuickMode ( 'class.yourwebservice.php' );
```

**If your webservice _needs more files_:**

```
require_once ( 'class.phpwsdl.php' );
PhpWsdl::RunQuickMode ( Array ( 'class.yourwebservice.php', 'class.yourcomplextype.php' ) );
```

Quick, isn't it?

But PhpWsdl can do a lot more for you. See the demos in the downloads for some examples.

## Features ##

  * parsing of WSDL definitions from comment blocks
  * creating WSDL even without definitions in comments
  * caching of the generated WSDL for more performance
  * support for complex types and arrays
  * create optimized or human readable WSDL
  * create WSDL with inline documentation
  * output HTML documentation
  * downloadable PDF documentation for your webservice including attached WSDL files and  PHP SOAP client proxy code
  * create PHP client proxy code
  * create JavaScript client proxy code
  * highly and easy extendable with hooks
  * support for adding more complex or simple type support with plugins
  * works with PHP SoapServer or any other SOAP server (NuSOAP or Zend f.e.)
  * easy to handle SOAP client included (works with PHPs native SoapClient)
  * fully documented source code
  * http Auth protection for your SOAP webservice
  * mixing global and class methods within one webservice
  * NuSOAP adapter
  * AJAX proxy to foreign SOAP webservices for JavaScript SOAP clients
  * run SOAP, XML RPC, JSON, http and REST webservices with the same code base

## Online demonstration ##

### Webservice HTML documentation output ###

At this location you see a demonstration of the HTML output from PhpWsdl. The documentation may be downloaded as PDF file with the WSDL files as inline attachments. This will help you to provide your webservice details well documented.

http://wan24.de/test/phpwsdl2/demo.php

### Webservice WSDL output ###

In this online demonstration the documentation is within the WSDL XML. If you show the source, the XML is formatted. But PhpWsdl can also produce WSDL without inline documentation and optimized for a better performance.

http://wan24.de/test/phpwsdl2/demo.php?WSDL

### Endpoint for testing ###

Use this location for your preferred SOAP client to test the compatibility of the WSDL produced by PhpWsdl:

http://wan24.de/test/phpwsdl2/demo.php

This demo location supports also the XML RPC, JSON, http and REST protocol.

### Sample auto-generated client proxy classes for PHP and JavaScript ###

**SOAP**:
http://wan24.de/test/phpwsdl2/demo.php?PHPSOAPCLIENT

**XML RPC**:
http://wan24.de/test/phpwsdl2/demo.php?PHPRPCCLIENT

**JSON**:
http://wan24.de/test/phpwsdl2/demo.php?PHPJSONCLIENT (PHP) or http://wan24.de/test/phpwsdl2/demo.php?JSJSONCLIENT (JavaScript)

**http**:
http://wan24.de/test/phpwsdl2/demo.php?PHPHTTPCLIENT

**REST**:
http://wan24.de/test/phpwsdl2/demo.php?PHPRESTCLIENT