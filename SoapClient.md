# Introduction #

The WSDL produced by PhpWsdl is independent from the used SOAP client. And PhpWsdl brings its own SOAP client solution.

# Details #

PhpWsdl wraps the native PHP SoapClient object. Example usage:

```
require_once('class.phpwsdlclient.php');
$client=new PhpWsdlClient('http://wan24.de/test/phpwsdl2/demo4.php?WSDL');
echo $client->SayHello('you');
```

But the client can do quiet more for you:

  * cache the target SOAP webservices WSDL
  * create a PHP SOAP client proxy class from a foreign webservice