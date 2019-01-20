---
title: Application flow - Core concepts
sb: sidebar/core-concepts.html
---

# Application flow

Peak application are Middlewares centric. You can resume the flow like this. A user proceed a HTTP Request, then it is forwarded to your application HTTP stack which resolve it and return a HTTP response whenever possible.

<img src="https://raw.githubusercontent.com/peakphp/docs/master/_pencils/request_response_flow.png" alt="Peak">

If a Middleware issue a HTTP response, the application HTTP stack will be stopped and the response returned.

There is two outcomes possible when the main HTTP stack is resolved:

 - It stop when a PSR-7 Response is returned.
 - It throw an exception because no response have been returned.


