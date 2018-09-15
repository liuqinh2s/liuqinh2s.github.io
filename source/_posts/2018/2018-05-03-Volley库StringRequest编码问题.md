---
title: Volley库StringRequest编码问题
date: 2018-05-03
categories: [Android, Volley库]
comments: true
---

最近在做的一个工作是：从一个URL地址读取一系列手机联系人资料，保持到手机通讯录里面。

项目使用了Volley库，FastJson库，遇到的一个bug是编码错误，FastJson在进行从String到bean的解析的时候，编码不正确导致的出错，这个时候，先检查了URL的编码是UTF-8的，然后Android Studio的程序源文件也是UTF-8的，那就只有一个可能了，网络传输过程中的编码错误。

google一下发现早就有人在stackoverflow上问过这个问题了：

>How to get Android Volley StringRequest GET to return responses in UTF-8 encoding

How can I get Android Volley StringRequest GET to return responses in UTF-8 encoding? It only seems to return responses in ISO-8859-1.

Is it possible to get it to accept a UTF-8 string?

StringRequest call HttpHeaderParser.parseCharset(response.headers) to get charset from response headers, is 'Content-Type' in headers don`t contains 'charset' the default charset return as 'ISO-8859-1'.

StringRequest.java

```Java
protected Response<String> parseNetworkResponse(NetworkResponse response) {
    String parsed;
    try {
        parsed = new String(response.data, HttpHeaderParser.parseCharset(response.headers));
    } catch (UnsupportedEncodingException var4) {
        parsed = new String(response.data);
    }

    return Response.success(parsed, HttpHeaderParser.parseCacheHeaders(response));
}
```

HttpHeaderParser.java

```Java
public static String parseCharset(Map<String, String> headers, String defaultCharset) {
    String contentType = headers.get(HTTP.CONTENT_TYPE);
    if (contentType != null) {
        String[] params = contentType.split(";");
        for (int i = 1; i < params.length; i++) {
            String[] pair = params[i].trim().split("=");
            if (pair.length == 2) {
                if (pair[0].equals("charset")) {
                    return pair[1];
                }
            }
        }
    }

    return defaultCharset;
}

/**
 * Returns the charset specified in the Content-Type of this header,
 * or the HTTP default (ISO-8859-1) if none can be found.
 */
public static String parseCharset(Map<String, String> headers) {
    return parseCharset(headers, HTTP.DEFAULT_CONTENT_CHARSET);
}
```

so, you can parse to 'UTF-8' in 2 ways:

1. tell your webServer to add 'Content-Type' with 'charset=UTF-8' in headers
2. Create a subclass of StringRequest and override parseNetworkResponse method

意思是定制一个`MyStringRequest`类，继承`StringRequest`类，重载`parseNetworkResponse`即可：

```Java
protected Response<String> parseNetworkResponse(NetworkResponse response) {
    String parsed;
    try {
        parsed = new String(response.data, "utf-8");
    } catch (UnsupportedEncodingException var4) {
        parsed = new String(response.data);
    }

    return Response.success(parsed, HttpHeaderParser.parseCacheHeaders(response));
}
```
