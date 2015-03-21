# Introduction #

A PHP SoapServer object.

# Details #

See the [PHP online documentation](http://www.php.net/manual/en/book.soap.php): PhpWsdl uses the PHP SoapServer per default.

But you may use any other SOAP server with PhpWsdl, too - NuSOAP or Zend for example.

The NuSOAP adapter is ready and tested, but C# clients won't work with NuSOAP as SOAP server.

The Zend adapter is still in progress.

I recommend using the native PHP SoapServer. It's running ok, is fast and doesn't need too much ressources.