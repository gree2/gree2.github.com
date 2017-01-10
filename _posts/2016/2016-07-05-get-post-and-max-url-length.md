---
layout: post
title: "get post and max url length"
description: ""
category: [web]
tags: [get, post, url]
---
{% include JB/setup %}


### max url length

1. keep urls under 2000 characters

1. ie8-2083 ie9-2083

1. search engines like urls < 2048

### cors

1. [cross origin http request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

1. [http://enable-cors.org/](http://enable-cors.org/) and [source code](https://github.com/monsur/enable-cors.org)

1. cors

    1. java code

            public class SimpleCorsFilter implements Filter {

                private static final Log logger = LogFactory.getLog(SimpleCorsFilter.class);

                public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
                    logger.info("cors filter start");
                    HttpServletResponse response = (HttpServletResponse) res;
                    response.setHeader("Access-Control-Allow-Origin", "*");
                    response.setHeader("Access-Control-Allow-Methods", "GET, POST, HEAD, PUT, DELETE");
                    response.setHeader("Access-Control-Max-Age", "36000");
                    response.addHeader("Access-Control-Allow-Headers",
                            "Origin, No-Cache, X-Requested-With, Content-Type, Verification-Hash, User-Id, User-Token");
                    chain.doFilter(req, res);
                    logger.info("cors filter stop");
                }

                public void init(FilterConfig filterConfig) {

                }

                public void destroy() {

                }
            }

    1. web.xml

                <filter>
                    <filter-name>cors</filter-name>
                    <filter-class>io.github.gree2.web.filter.SimpleCorsFilter</filter-class>
                </filter>
                <filter-mapping>
                    <filter-name>cors</filter-name>
                    <url-pattern>/*</url-pattern>
                </filter-mapping>

### csrf

1. [cross site request forgery](http://www.squarefree.com/securitytips/web-developers.html#CSRF)

### reference

1. [what-is-the-maximum-length-of-a-url-in-different-browsers](http://stackoverflow.com/questions/417142/what-is-the-maximum-length-of-a-url-in-different-browsers)

1. [maximum-length-of-http-get-request](http://stackoverflow.com/questions/2659952/maximum-length-of-http-get-request)

1. [maximum url length is 2,083 characters in internet explorer](https://support.microsoft.com/en-us/kb/208427)

            Microsoft Internet Explorer has a maximum uniform resource locator (URL) length of 2,083 characters.
            Internet Explorer also has a maximum path length of 2,048 characters.
            This limit applies to both POST request and GET request URLs.

            If you are using the GET method,
            you are limited to a maximum of 2,048 characters,
            minus the number of characters in the actual path.

            POST method is not limited by the size of the URL for submitting name/value pairs.
            These pairs are transferred in the header and not in the URL.

1. [how-long-of-a-url-can-internet-explorer-9-take](http://stackoverflow.com/questions/3721034/how-long-of-a-url-can-internet-explorer-9-take)