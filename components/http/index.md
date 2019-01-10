### Peak/Http


Stack represent a pipeline of middlewares designed to handle an http request and/or return a reponse. 

Any PSR-15 middleware can be added to a stack. 
A stack itself can be added to a parent stack.

It also support simple closure but behind the scene, closure are bridged to a valid Psr\Http\Server\MiddlewareInterface

