---
title: Java Servlet 学习笔记
date: 2018-05-01
categories: [Java, Java web]
comments: true
---

>The difference between servlets and JSP is that servlets typically embed HTML inside Java code, while JSPs embed Java code in HTML.

不过现在都是用MVC框架了。

## life cycle of a servlet

Three methods are central to the life cycle of a servlet. These are `init()`, `service()`, and `destroy()`. They are implemented by every servlet and are invoked at specific times by the server.

- the web container initializes the servlet instance by calling the init() method, passing an object implementing the `javax.servlet.ServletConfig` interface. This configuration object allows the servlet to access name-value initialization parameters from the web application.
- Each request is serviced in its own separate thread. The web container calls the service() method of the servlet for every request. The developer of the servlet must provide an implementation for these methods.
- Finally, the web container calls the destroy() method that takes the servlet out of service. The destroy() method, like init(), is called only once in the lifecycle of a servlet.

The following is a typical user scenario of these methods.

1. Assume that a user requests to visit a URL.
	- The browser then generates an HTTP request for this URL.
	- This request is then sent to the appropriate server.
2. The HTTP request is received by the web server and forwarded to the servlet container.
	- The container maps this request to a particular servlet.
	- The servlet is dynamically retrieved and loaded into the address space of the container.
3. The container invokes the `init()` method of the servlet.
	- **This method is invoked only when the servlet is first loaded into memory.**
	- It is possible to pass initialization parameters to the servlet so that it may configure itself.
4. The container invokes the `service()` method of the servlet.
	- This method is called to process the HTTP request.
	- The servlet may read data that has been provided in the HTTP request.
	- The servlet may also formulate an HTTP response for the client.
5. The servlet remains in the container's address space and is available to process any other HTTP requests received from clients.
	- **The service() method is called for each HTTP request.**
6. The container may, at some point, decide to unload the servlet from its memory.
	- The algorithms by which this decision is made are specific to each container.
7. The container calls the servlet's destroy() method to relinquish any resources such as file handles that are allocated for the servlet; important data may be saved to a persistent store.
8. The memory allocated for the servlet and its objects can then be garbage collected.

## Example

The following example servlet prints how many times its `service()` method was called.

Note that `HttpServlet` is a subclass of `GenericServlet`, an implementation of the `Servlet` interface.

The `service()` method of `HttpServlet` class dispatches requests to the methods `doGet()`, `doPost()`, `doPut()`, `doDelete()`, and so on; according to the HTTP request. In the example below `service()` is overridden and does not distinguish which HTTP request method it serves.

```Java
import java.io.IOException;
 
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletLifeCycleExample extends HttpServlet {
 
    private int count;
 
    @Override
    public void init(final ServletConfig config) throws ServletException {
        super.init(config);
        getServletContext().log("init() called");
        count = 0;
    }
 
    @Override
    protected void service(final HttpServletRequest request, final HttpServletResponse response) throws ServletException, IOException {
        getServletContext().log("service() called");
        count++;
        response.getWriter().write("Incrementing the count to " + count);
    }
 
    @Override
    public void destroy() {
        getServletContext().log("destroy() called");
    }
}
```

>javax是什么？

java和javax都是Java的API(Application Programming Interface)包，java是核心包，**javax的x是extension的意思，也就是扩展包**。java类库是java发布之初就确定了的基础库，而javax类库则是在上面增加的一层东西，就是为了保持版本兼容要保存原来的，但有些东西有了更好的解决方案，所以，就加上些，典型的就是awt(Abstract Windowing ToolKit) 和swing。