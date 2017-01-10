---
layout: post
title: "java automate website login"
description: ""
category: [java]
tags: [auto, HttpURLConnection, CookieHandler, CookieManager]
---
{% include JB/setup %}


### code

1. class define

    1. ctor

            private HttpURLConnection conn;

            public Auth() {
                // make sure cookies is turn on
                CookieHandler.setDefault(new CookieManager());
            }

    1. send post

            /**
             * send post request
             * @param url url
             * @param params params
             * @return response
             * @throws Exception
             */
            public String sendPost(String url, String params) throws Exception {

                URL obj = new URL(url);
                conn = (HttpURLConnection) obj.openConnection();

                // Acts like a browser
                conn.setUseCaches(false);
                conn.setRequestMethod("POST");
                // conn.setRequestProperty("Host", "your.api.host");
                // conn.setRequestProperty("User-Agent", USER_AGENT);
                // conn.setRequestProperty("Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8");
                // conn.setRequestProperty("Accept-Language", "en-US,en;q=0.5");

                // conn.setRequestProperty("Connection", "keep-alive");
                // conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
                // conn.setRequestProperty("Content-Length", Integer.toString(params.length()));

                conn.setDoOutput(true);
                conn.setDoInput(true);

                // Send post request
                DataOutputStream wr = new DataOutputStream(conn.getOutputStream());
                wr.writeBytes(params);
                wr.flush();
                wr.close();

                // int responseCode = conn.getResponseCode();
                // System.out.println("\nSending 'POST' request to URL : " + url);
                // System.out.println("Post parameters : " + params);
                // System.out.println("Response Code : " + responseCode);

                return getResponse();
            }

    1. send get

            /**
             * sent get request
             * @param url url
             * @param params params
             * @return response
             * @throws Exception
             */
            public String sendGet(String url, String params) throws Exception {

                URL obj = new URL(url);
                conn = (HttpURLConnection) obj.openConnection();

                // default is GET
                conn.setRequestMethod("GET");

                conn.setUseCaches(false);

                // act like a browser
                String USER_AGENT = "Mozilla/5.0";
                conn.setRequestProperty("User-Agent", USER_AGENT);
                conn.setRequestProperty("Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8");
                conn.setRequestProperty("Accept-Language", "en-US,en;q=0.5");

                conn.setDoOutput(true);
                conn.setDoInput(true);

                // Send post request
                DataOutputStream wr = new DataOutputStream(conn.getOutputStream());
                wr.writeBytes(params);
                wr.flush();
                wr.close();

               return getResponse();

            }

    1. get response

            /**
             * get response
             * @return response
             * @throws IOException
             */
            private String getResponse() throws IOException {
                BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                String line = br.readLine();
                StringBuilder sb = new StringBuilder();
                while (line != null) {
                    sb.append(line);
                    line = br.readLine();
                }
                return sb.toString();
            }

1. usage

        // 0. auth
        Auth auth = new Auth();

        // 1. login first
        String loginUrl = "http://...";
        String loginParams = "username=...&password=...";
        auth.sendPost(loginUrl, loginParams);

        // 2. post example
        String apiUrl = "http://...";
        String apiParams = "...";
        System.out.println(auth.sendPost(apiUrl, apiParams));

        // 3. get example
        apiUrl = "http://...";
        apiParams = "...";
        System.out.println(auth.sendGet(apiUrl, apiParams));

### reference

1. [how-to-automate-login-a-website-java-example](https://www.mkyong.com/java/how-to-automate-login-a-website-java-example/)
