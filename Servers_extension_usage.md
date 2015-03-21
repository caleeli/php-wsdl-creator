# Introduction #

PhpWsdlServers enabled you to run a JSON, http, XML RPC and REST webservice
with one codebase.

# Details #

If you are already running a SOAP webservice with PhpWsdl, you can extend
it by simply installing the PhpWsdlServers extension. The download package
contains some working examples.

## JSON webservice ##

To serve a running PhpWsdl SOAP webservice as JSON webservice, you don't need
to modify your code. It will run out of the box.

To get a PHP JSON client proxy for your webservice, call this URI:

```
http://your-server.com/webservice.php?PHPJSONCLIENT
```

To get a JavaScript JSON client proxy for your webservice, call this URI:

```
http://your-server.com/webservice.php?JSJSONCLIENT
```

To get a compressed JavaScript JSON client proxy for your webservice, call this
URI:

```
http://your-server.com/webservice.php?JSJSONCLIENT&min
```

The webservice request is done by giving a JSON array/object as parameter
"json" or "JSON" with POST or GET:

```
{
	"call":"MethodName",
	"param":[
		"Parameter1",
		123,
		true
	]
}
```

This example needs the webservice to export the method "MethodName" that can
be called with three parameters: A string, an integer and a boolean value.
That's all :)

It's possible to attach the JavaScript clients to the PDF documentation, too.
But you should know that users won't be able to save them with Adobe Acrobat
Reader since the security permissions won't allow that. If you want to force
the JavaScript client attachments in PDF:

```
PhpWsdlServers::$AttachJsInPdf=true;
```

**Note**: To enable PDF attachments in the PDF documentation download you need
to set up a valid HTML2PDF license key in PhpWsdl.

To fully disable serving JSON:

```
PhpWsdlServers::$EnableJson=false;
```

## http webservice ##

To serve a running PhpWsdl SOAP webservice as http webservice, you don't need
to modify your code. It will run out of the box.

To get a PHP http client proxy for your webservice, call this URI:

```
http://your-server.com/webservice.php?PHPHTTPCLIENT
```

A client needs to submit the name of the exported method in the parameter
"call". Every parameter that is required by this method needs to be submitted
with the parameter name. Requests can be done with the GET or POST http method.

For example, if your webservice exports the method "MethodName" that requires
three parameters ($string is a string, $int is an integer, $bool is an boolean
value) a GET request URI would look like this:

```
http://your-server.com/webservice.php?call=MethodName&string=Parameter1&int=123&boolean=1
```

If a parameters type is not listed in the PhpWsdl::$BasicTypes array, it needs
JSON encoding. The same for the response: If the return values type is not a
basic type, it will be returned JSON encoded! A parameter can't be NULL. The
response may be NULL if it requires JSON encoding.

To fully disable serving http:

```
PhpWsdlServers::$EnableHttp=false;
```

## REST webservice ##

To serve a running PhpWsdl SOAP webservice as REST webservice, you don't need
to modify your code. It will run out of the box.

To get a PHP REST client proxy for your webservice, call this URI:

```
http://your-server.com/webservice.php?PHPRESTCLIENT
```

But since REST offers some more features, you can extend your webservices with
special REST configuration by using the @pw\_rest keyword in a comment block
for an exported method:

```
/**
 * Summarize two numbers
 *
 * @param int $a A number
 * @param int $b Another number
 * @return int The result of $a+$b
 * @pw_rest GET /sum/:a/:b Summarize two numbers
 */
public function Add($a,$b=0){
	return $a+$b;
}
```

As you can see you have to use ":" as identifier for a named parameter. The
parameter order may be different from the function declaration.

Of course the method will be exported for SOAP, JSON and http clients, too.
But with the REST protocol you can now access the method like this:

```
http://your-server.com/webservice.php/sum/1/2
http://your-server.com/webservice.php/sum/1
http://your-server.com/webservice.php/Add/1/2
http://your-server.com/webservice.php/Add/1
```

This will result in the following output:

```
3
1
3
1
```

As you can see, the method will be exported with a default REST path that has
this syntax:

```
/[method]/[parameter1]/[parameter2]/...
```

[method](method.md) is the case sensitive method name. All parameters are added in the
order of the declaration in the comment block.

A client has to provide every parameter that is required to call the method.
In this example the method "Add" may be called with one or two parameters in a
PHP script - the same for calling with REST.

If a method has more that one parameter, the last parameter may be submitted
in the POST http request body.

By the way: REST works with global methods (=not in a class), too.

To fully disable serving REST:

```
PhpWsdlServers::$EnableRest=false;
```

## XML RPC webservice ##

To serve a running PhpWsdl SOAP webservice as XML RPC webservice, you don't
need to modify your code. It will run out of the box.

To get a PHP XML RPC client proxy for your webservice, call this URI:

```
http://your-server.com/webservice.php?PHPRPCCLIENT
```

Per default the XML RPC webservice will consume the given parameters as
simple array. To enable the support for named parameters:

```
PhpWsdlServers::$EnableRpcNamedParameters=true;
```

If named parameters are enabled you have to use named parameters. There is no
way to support both calling methods.

To fully disable serving XML RPC:

```
PhpWsdlServers::$EnableRpc=false;
```

## Enable client cache ##

Per default the client cache will be disabled by sending some http headers.
But if you want your clients to be enabled caching your webservices response,
you can do that with:

```
PhpWsdlServers::$DisableClientCache=false;
```