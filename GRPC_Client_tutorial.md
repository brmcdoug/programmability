## GRPC client command line tutorial and examples

We recently opensourced a python based GRPC client designed to interact with IOS-XR 6.x:

[https://xrdocs.github.io/programmability/tutorials/2016-11-03-grpc-in-python-for-ios-xr/](https://xrdocs.github.io/programmability/tutorials/2016-11-03-grpc-in-python-for-ios-xr/)

[https://github.com/cisco-grpc-connection-libs/ios-xr-grpc-python](https://github.com/cisco-grpc-connection-libs/ios-xr-grpc-python)

The client now includes CLI options and a number of additional sample json snips to include with your RPCs.  This tutorial walks through the use of the GRPC Client CLI and examples.

What you'll need:

1. Management station/laptop: python, python-dev (ubuntu), and 'pip install grpcio'
2. One or more IOS-XR routers running 6.x code.  Note: the client has been tested mostly on 6.1.1
3. Configure GRPC server on your router(s)

```
grpc

 port 57400
```
 
The CLI doesn't currently support TLS encryption, so we've left that option disabled.



Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
