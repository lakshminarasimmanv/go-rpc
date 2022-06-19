# Go RPC

Remote Procedure Call (RPC) is a protocol that one program can use to request a service from a program located in another computer on a network without having to understand the network's details.

Go's rpc package makes it easy to write such services.

The rpc package provides access to the exported methods of an object across a network or other I/O connection. A server registers an object, making it visible as a service with the name of the type of the object. After registration, exported methods of the object will be accessible remotely. A server may register multiple objects (services) of different types but it is an error to register multiple objects of the same type.

**Only methods that satisfy these criteria will be made available for remote access; other methods will be ignored:**

- The method's type is exported.
- The method is exported.
- The method has two arguments, both exported (or builtin) types.
- The method's second argument is a pointer.
- The method has return type error.
- In effect, the method must look schematically like

``func (t *T) MethodName(argType T1, replyType *T2) error``
where T, T1 and T2 can be marshaled by encoding/gob.

The method's first argument represents the arguments provided by the caller; the second argument represents the result parameters to be returned to the caller. The method's return value, if non-nil, is passed back as a string that the client sees as if created by errors.New. If an error is returned, the reply parameter will not be sent back to the caller.

**For example, the method**

``func (t *T) Scale(factor float64, result *float64) error``
can be called via a client as Scale(ctx, &args, &reply), where args.Factor contains the factor and reply will contain the result when Scale returns.

The server may handle requests on a single connection by calling ServeConn. More typically it will create a network listener and call Accept or, for an HTTP listener, HandleHTTP and http.Serve.

A client wishing to use the service establishes a connection and then invokes the desired method, providing the appropriate parameters. The client and server then communicate by sending messages using that connection and performing marshaling and unmarshaling of the parameters and results. The server replies with a response message. If an error occurs during the execution of the remote method, the error is returned by the server as is. If the connection is closed prematurely, the server returns an error.

The ``net/rpc`` package is frozen and is not accepting new features. The new package to use is ``net/rpc/jsonrpc``.

## How the above program works?

1. The server registers a value of type Arith using the name "Arith".
2. The client looks up the same name and sees that it refers to a value of type Arith.
3. The client calls the method Multiply on that value, passing the arguments 7 and 8.
4. The server receives the call and executes the body of the method.
5. The server returns the result, which the client prints.

## Usage

**Server:**

``go run rpc_server.go``

**Client:**

``go run rpc_client.go``