---
title: JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 1
date: 2019-04-28 16:51:00 +0900
categories: [FRAMEWORK,SPRING,JSP-SERVLET]
tags: [jsp-servlet,네아로,API]
---

[목차테스트1](https://jeongdoc.com/posts/leetcode-easy-singlenumber/)

# 들어가며
 spring이라는 정말 많이 쓰이는 JAVA 플랫폼 프레임워크이 있지만, spring 이전에 JSP-SERVLET(+MVC)가 있었습니다.
이를 공부하는 것은 spring의 구조, 등장배경 등 이해에 도움이 될 것이라고 생각합니다.
그렇기에 공부의 의미로 JSP-SERLVET을 이용하였고 네아로API 연동해보았던 것을 간단하게나마 정리해보고자 합니다.
JSP와 Servlet만을 이용하여 접근토큰(access_token) 가져오는 방법까지만 서술합니다.

# 1. 네아로를 이용하기 위한 사전 작업
1. 네이버 개발자 센터 - 네아로
[네이버 개발자 센터로 이동](https://developers.naver.com/main)
네이버아이디로로그인(이하 네아로)은 네이버 개발자 센터에서 신청하면 됩니다.
위 링크를 이용하면 사이트로 바로 이동합니다.

# 2. API 신청
API 신청을 하는 과정이나 신청 이후에 아래와 같은 페이지를 볼 수 있습니다.
![""](https://jeongdoc.com/assets/img/2019/04/naverloginapi1.png)

사용 API항목에서 자신의 사이트 이용시 필수 혹은 추가로 받고자 하는 정보에 체크를 합니다.
다만, 사용자가 네이버 자체에서 정보제공 동의를 하지 않은 항목의 경우엔 필수로 받아온다고 체크를 해도 사용할 수 없습니다.
따라서 이 부분은 사이트 성향에 따라 원하는 대로 구성하면 됩니다.
저는 이름(실명)만 추가 입력을 받았습니다.

![""](https://jeongdoc.com/assets/img/2019/04/naverloginapi2.png)
아래쪽엔 위와 같은 페이지가 보입니다.
서비스 URL과 콜백URL을 필수로 기입해야 합니다. 서비스 URL은 네아로 버튼이 노출될 페이지의 주소를 적으면 됩니다.
네아로 버튼이라고 했지만 사실상 회원가입이나 로그인버튼이며, 네이버로 로그인 버튼이 띄워질 페이지의 URL을 적으면 되겠습니다.

소셜로그인API이용시 token이라는 것을 접하게 됩니다. 
토큰엔 접근토큰(access_token), 갱신토큰(refrech_token)이 있습니다. 
로그인API에서의 토큰이란, 세션과 비슷한 역할을 한다고 보면 됩니다. 
이 접근 토큰이 있어야만 네아로로 로그인을 할 수 있습니다. 
따라서 접근 토큰을 가져와야 하는데 이 토큰을 받을 페이지를 콜백페이지로 생각하면 됩니다.
즉 접근토큰을 받을 페이지의 url을 callback url에 기입하면 됩니다.
참고로, 네아로는 localhost는 지원하지 않으므로 127.0.0.1이나 자신의 ip주소를 이용합시다.

![""](https://jeongdoc.com/assets/img/2019/04/naverloginapi3.png)
정상적으로 API사용신청이 완료 되었다면 내 애플리케이션(위에서 지은 이름이 아래에 뜬다)항목에서 Client ID 와 Client Secret 정보를 확인 할 수 있습니다.
이 정보는 서비스url 페이지와 callback url 페이지에서 사용해야하기 때문에 어디에 적어놓거나 페이지를 띄워놓고 작업하길 추천합니다.

여기까지 완료했다면 네아로 사용을 위한 세팅은 끝입니다.

# 3. 서비스 url, callback url

[네이버 개발자 센터 API명세페이지로 이동](https://developers.naver.com/docs/login/api)

 네이버 개발자 센터에 보면 API명세가 있습니다. 이 명세를 참고해서 서비스와 콜백페이지를 구성하면 됩니다.
API 호출 예제를 잘 표기해놨는데 javascript, jsp, php, node.js, asp.net 이렇게 다섯 가지의 예제가 표기되어 있습니다.
여기선 JSP-SERVLET을 이용할 것이므로 JSP 호출예제를 그대로 가져오면 됩니다.

``` jsp
네이버 로그인 접근토큰 획득 예제는 2개로 구성되어 있습니다. (naverlogin.jsp, callback.jsp)
1. naverlogin.jsp
<%@ page import="java.net.URLEncoder" %>
<%@ page import="java.security.SecureRandom" %>
<%@ page import="java.math.BigInteger" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
    네이버로그인
  <%
    String clientId = "YOUR_CLIENT_ID";//애플리케이션 클라이언트 아이디값";
    String redirectURI = URLEncoder.encode("YOUR_CALLBACK_URL", "UTF-8");
    SecureRandom random = new SecureRandom();
    String state = new BigInteger(130, random).toString();
    String apiURL = "https://nid.naver.com/oauth2.0/authorize?response_type=code";
    apiURL += "&client_id=" + clientId;
    apiURL += "&redirect_uri=" + redirectURI;
    apiURL += "&state=" + state;
    session.setAttribute("state", state);
 %>
 
2. callback.jsp
<%@ page import="java.net.URLEncoder" %>
<%@ page import="java.net.URL" %>
<%@ page import="java.net.HttpURLConnection" %>
<%@ page import="java.io.BufferedReader" %>
<%@ page import="java.io.InputStreamReader" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
    네이버로그인
  <%
    String clientId = "YOUR_CLIENT_ID";//애플리케이션 클라이언트 아이디값";
    String clientSecret = "YOUR_CLIENT_SECRET";//애플리케이션 클라이언트 시크릿값";
    String code = request.getParameter("code");
    String state = request.getParameter("state");
    String redirectURI = URLEncoder.encode("YOUR_CALLBACK_URL", "UTF-8");
    String apiURL;
    apiURL = "https://nid.naver.com/oauth2.0/token?grant_type=authorization_code&";
    apiURL += "client_id=" + clientId;
    apiURL += "&client_secret=" + clientSecret;
    apiURL += "&redirect_uri=" + redirectURI;
    apiURL += "&code=" + code;
    apiURL += "&state=" + state;
    String access_token = "";
    String refresh_token = "";
    System.out.println("apiURL="+apiURL);
    try {
      URL url = new URL(apiURL);
      HttpURLConnection con = (HttpURLConnection)url.openConnection();
      con.setRequestMethod("GET");
      int responseCode = con.getResponseCode();
      BufferedReader br;
      System.out.print("responseCode="+responseCode);
      if(responseCode==200) { // 정상 호출
        br = new BufferedReader(new InputStreamReader(con.getInputStream()));
      } else {  // 에러 발생
        br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
      }
      String inputLine;
      StringBuffer res = new StringBuffer();
      while ((inputLine = br.readLine()) != null) {
        res.append(inputLine);
      }
      br.close();
      if(responseCode==200) {
        out.println(res.toString());
      }
    } catch (Exception e) {
      System.out.println(e);
    }
  %>
```
 이 부분을 서비스페이지로 이용할 파일에 각각 붙여넣기 합니다.
naverlogin.jsp와 callback.jsp는 예제 이름으로써, 자신이 만든 로그인페이지의 jsp이름이 login.jsp이고, 콜백 페이지가 cback.jsp라면 naverlogin.jsp 코드는 login.jsp에 callback.jsp는 cback.jsp에 붙여넣기 하면 됩니다.
여기까지 마쳤다면 토큰을 가져오는 환경조성은 끝났습니다.
저는 callback.jsp를 따로 만들지 않고 servlet에 별도로 작업하였습니다.
이는 다음에 되짚어볼 것입니다.
다음 글에서 본격적으로 access_token을 다루는 법을 작성합니다.