---
title: JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 2
date: 2019-04-30 12:16:00 +0900
categories: [FRAMEWORK,SPRING,JSP-SERVLET]
tags: [jsp-servlet,네아로,API]
---

### 네아로 API 사용하기 게시글 목록
> [JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 1](https://jeongdoc.com/posts/naverloginapi1)
> [JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 2](https://jeongdoc.com/posts/naverloginapi2)
> [JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 3](https://jeongdoc.com/posts/naverloginapi3)

### 들어가며

[네이버아이디로로그인 API (네아로) 사용하기 - 1 링크](https://jeongdoc.com/posts/naverloginapi1)

 1편에서는 네이버 개발자 센터에 있는 API 사용법을 적용하는 것을 작성해보았습니다. 
2편에서는 callback페이지를 어떻게 구성했는지 알아봅시다.
callback.jsp로 예시가 나와있었지만 저는 servlet에 작성했습니다. 이를 기반으로 글을 작성하려 합니다.

### 1. callback 페이지 구성 (servlet 구성)
 우선 controller 단(혹은 자신이 구성한 구조에 따라서 적절히 구성) 네아로를 처리할 serlvet을 하나 만듭니다. 
클래스나 메소드 명칭은 어떤 역할을 하는지 바로 알 수 있는 명칭이 좋습니다.

``` java
package member.controller;
 
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
 
@WebServlet("/[url 맵핑값]")
public class NaverLoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
  /**
   * @see HttpServlet#HttpServlet()
   */
  public NaverLoginServlet() {
      super();
      // TODO Auto-generated constructor stub
  }
 
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 네이버 로그인 처리 컨트롤러
  }
    /**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
 
}
```

 대충 위와 같은 식으로 서블릿이 생성될 것입니다. 
코드는 doGet 메소드에서 전부 작성하였습니다. 
앞서 설명하였지만, 이름을 지어줄 때 무슨 역할을 하는 서블릿인지 알 수 있도록 지어주는 것이 좋습니다. 
애매하게 지으면 무조건 나중에 헷갈립니다.
doGet메소드에 이 서블릿이 무슨 역할을 하는지 간단하게 주석도 달아주었습니다.

### 2.callback.jsp의 코드 작성

 이제 예시 callback.jsp 코드를 적용해봅시다.
doGet 메소드 안에 코드를 작성하면 됩니다.

``` java
String clientId = "[1편에서 얻은 clientId값 입력]";
String clientSecret = "[1편에서 얻은 clientSecret값 입력]"; 
String code = request.getParameter("code");
String state = request.getParameter("state");
String redirectURI = URLEncoder.encode("[로그인 후 보이게 할 페이지 url]","UTF-8");
		
StringBuffer apiURL = new StringBuffer();
apiURL.append("https://nid.naver.com/oauth2.0/token?grant_type=authorization_code&");
apiURL.append("client_id=" + clientId);
apiURL.append("&client_secret=" + clientSecret);
apiURL.append("&redirect_uri=" + redirectURI);
apiURL.append("&code=" + code);
apiURL.append("&state=" + state);
String access_token = "";
String refresh_token = ""; //나중에 이용합시다
		
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
    .
    .
    .
```

네이버 개발자 센터에 예시로 작성되어있는 API호출 예제(callback.jsp)의 코드를 싹 긁어오면 위와 같습니다.
이렇게 하면 access_token을 가져올 준비가 끝난 것입니다.
또, 위의 코드에 추가해주어야할 코드가 있습니다.
br.close(); 아래의 if문 안에 추가해줍니다.

``` java
//위의 코드
 while ((inputLine = br.readLine()) != null) {
        res.append(inputLine);
      }
      br.close();
      if(responseCode==200) {
        out.println(res.toString());
       /*
      	이 부분에 코드 추가
      */
      }
      
//추가한 후의 코드
while ((inputLine = br.readLine()) != null) {
	res.append(inputLine);
}
 br.close();
if(responseCode==200) {
	System.out.println(res.toString());
	JSONParser parsing = new JSONParser();
	Object obj = parsing.parse(res.toString());
	JSONObject jsonObj = (JSONObject)obj;
		        
	access_token = (String)jsonObj.get("access_token");
	refresh_token = (String)jsonObj.get("refresh_token");
}
// 여기까지하면 response 리다이렉트하는 페이지에서 토큰값을 볼 수 있음
```
responseCode==200이라는건 오류가 없이 정상작동됐을 경우를 뜻합니다. 
즉 200코드라면(오류없이 정상적으로 작동했다면) if문 안의 코드를 실행하라는 의미죠.

그럼 if문 안에 작성할 코드는 어떤 것일까요?
네아로를 사용하고자할때 끊임없이 또 꼼꼼하게 API명세를 보면서 작업해야합니다.
![""](https://jeongdoc.com/assets/img/2019/04/naverloginapi2-1.png)

와인색 네모로 표시한 부분을 보면 접근토큰 발급/갱신/삭제요청시 출력 포맷이 json인걸 알 수 있습니다.
callback.jsp의 access_token 선언부를 보면 String으로 초기화해준 것을 볼 수 있습니다.
즉 출력 포맷이 json인 access_token을 String형식으로 받아야한다는 의미라고 생각하면 됩니다.
그렇기에 JSONParser를 이용해 Object로 업캐스팅을 한 후 다시 JSONObject로 다운캐스팅을 해서 가져와야합니다.

물론 업캐스팅 다운캐스팅 과정을 한 줄의 코드로 작성할 수도 있지만 저는 형변환을 한꺼번에 하는 것을 선호하지 않는 편입니다.
따라서 업캐스팅, 다운캐스팅 각각을 나누어서 해주었습니다.
Json은 맵형식이고 맵에서 값을 꺼낼때처럼 Json값도 꺼내주면 됩니다.
get메소드를 이용하여 최종적으로 token값을 꺼내면 끝입니다.
access_token은 String으로 생성해주었으므로 String으로 형변환하여 access_token변수에 담으면 됩니다.
 
***    

여기까지가 네아로를 이용하여 로그인API명세를 바탕으로 작성한 코드입니다.
access_token은 말그대로 접근 토큰인데 해당 사용자의 정보에 접근하려면 필요한 토큰입니다.
사용자의 정보 사용 요청을 위한 것이라고 이해하면 쉽습니다.
따라서 access_token을 정상적으로 발급받지 못한다면 네이버로 가입하는 회원의 정보를 사용할 수 없습니다.

2편까지 작업이 잘 되었다면 로그인요청까지 정상적으로 이루어진 것이라고 보면 됩니다.
네아로로 로그인을 한 사용자의 정보를 가져오는 법은 다음 글, 즉 3편에서 다룹니다.