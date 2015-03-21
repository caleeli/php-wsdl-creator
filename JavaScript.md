# Introduction #

I prefer using http://code.google.com/p/soa2ui/ as JavaScript SOAP client. But requesting foreign SOAP webservices won't work with JavaScript because of security restrictions of the browser. PhpWsdl has a solution for you.

# Details #

Example PHP code:

```
require_once('class.phpwsdlajax.php');
PhpWsdlAjax::RunForwarder('http://www.webservicex.net/geoipservice.asmx');
```

These two lines of code will serve the foreign webservice for your AJAX SOAP clients. The proxy will simply forward any SOAP request to another SOAP webservice.