# Introduction #

Whenever you want to make SOAP requests from an AJAX SOAP client, you'll note
that AJAX requests are restricted to the server that serves the SOAP client.
To make an AJAX request to a foreign server, you need a proxy.

# Details #

PhpWsdlAjax provides two methods to act as proxy.

## A simple forwarder ##

This will simply forward all requests to another URI:

```
require_once('class.phpwsdlajax.php');
PhpWsdlAjax::RunForwarder('http://target-server.com/');
```

The parameter is the URI to the target SOAP endpoint.

## A real SOAP proxy ##

PhpWsdlAjax can also act as a real SOAP webservice proxy:

```
require_once('class.phpwsdlajax.php');
PhpWsdlAjax::RunProxy('http://target-server.com/?WSDL');
```

The parameter is the URI to the target WSDL.

**Note**: The proxy can only handle webservices that have WSDL like the PhpWsdl
framework can produce it.