---
title: JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 3
date: 2019-05-14 15:31:00 +0900
categories: [FRAMEWORK,SPRING,JSP-SERVLET]
tags: [jsp-servlet,네아로,API]
---

### 네아로 API 사용하기 게시글 목록
> [JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 1](https://jeongdoc.com/posts/naverloginapi1)<br>
> [JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 2](https://jeongdoc.com/posts/naverloginapi2)<br>
> [JSP-SERVLET 네이버아이디로로그인 API (네아로) 사용하기 - 3](https://jeongdoc.com/posts/naverloginapi3)<br>

### 들어가며

[네이버아이디로로그인 API (네아로) 사용하기 - 1 링크](https://jeongdoc.com/posts/naverloginapi1)<br>
[네이버아이디로로그인 API (네아로) 사용하기 - 2 링크](https://jeongdoc.com/posts/naverloginapi2)<br>
 2편에서는 API명세에 따라 callback.jsp의 내용을 재구성하고 access_token값을 추출하는 것까지 했었습니다.<br>
그러면 이제 네아로로 로그인을 한 사용자의 정보를 가져오는 법을 알아봅시다.

### 1. api명세에 따라 url 생성
``` java
if(access_token != null) { // access_token을 잘 받아왔다면

	try {

    	// 이 안에 코드 작성

    } catch (Exception e) {

    	e.printStackTrace().

    }

}
```
 try 블럭 안에 남은 코드를 전부 작성합니다.<br>
아래부터는 try 블럭 안에 들어갈 코드라고 생각하고 읽어가시면 됩니다.

``` java
String apiurl = "https://openapi.naver.com/v1/nid/me";
URL url = new URL(apiurl);
HttpURLConnection con = (HttpURLConnection)url.openConnection();
con.setRequestMethod("GET");
con.setRequestProperty("Authorization", header);
int responseCode = con.getResponseCode();
BufferedReader br;

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
```

 api명세에 따라 작성하면 됩니다.<br>
apiurl도 제공해주는 것이고 오타없이 따라서 잘 작성합시다.<br>
토큰을 이용해 url을 생성하는 코드로서 네아로 뿐만이 아니라 구글, 카카오 같은 여타 소셜로그인 api도 이와 같은 방식으로 구성해주시면 됩니다.<br>
당연히 여기서 오타나 값을 잘못 받아오면 에러가 발생합니다.<br>

### 2. 정보 가져오기
 이제 네아로로 로그인한 사용자의 정보를 가져와봅시다.<br>
2편에서도 줄곧 강조하였듯, 네이버 개발자 센터에 있는 api명세를 꼼꼼히 읽어보면서 작업합니다.<br>
처음 사용하면 낯선 것이 당연하겠지만 결국 답은 api명세에 있으니 api명세를 다른 창에 띄워두고 작업하시길 추천드립니다.<br>

![""](https://jeongdoc.com/assets/img/2019/05/naverloginapi3-1.png)
 네이버 개발자 센터의 api명세쪽을 훑다보면 위와 같은 부분을 볼 수 있습니다.<br>

붉은색 네모상자로 표기되어있는 부분에 집중해봅시다.<br>
이 '필드'들을 이용하면 사용자 정보를 가져올 수 있습니다.<br>

``` java
JSONParser parsing = new JSONParser();
Object obj = parsing.parse(res.toString());
JSONObject jsonObj = (JSONObject)obj;
JSONObject resObj = (JSONObject)jsonObj.get("response");
 
//왼쪽 변수 이름은 원하는 대로 정하면 된다. 
//단, 우측의 get()안에 들어가는 값은 와인색 상자 안의 값을 그대로 적어주어야 한다.
String naverCode = (String)resObj.get("id");
String email = (String)resObj.get("email");
String name = (String)resObj.get("name");
String nickName = (String)resObj.get("nickname");
```

 위의 코드는 붉은색 네보로 표기한 '필드'를 이용한 것입니다.<br>
위와 같이 구성하시면 네아로 사용자의 정보를 이용할 수 있습니다.<br>
db에 insert를 할 수도 있고 사용자 정보 수정시 update를 할 수도 있습니다. db에 insert하거나 update하는 것은 매우 기본적인 것이므로 생략합니다.

 다음으로, 위의 코드에 이어 insert 혹은 update 코드를 작성해주면 됩니다.<br>
주의할 점은 주석으로도 써놓았지만 get()에 위 사진에 네모쳐놓은 값을 오타없이 그대로 써주어야 한다는 점입니다.<br>
naver에서 정해놓은 규칙이므로 지키지 않으면 당연히 값을 가져올 수 없습니다.<br>

 단, 사용자가 네이버 자체에 정보제공을 동의하지 않은 항목은 저렇게 해도 값을 가져올 수 없습니다. <br>
따라서 이름과 같은 것은 따로 입력을 받게끔 처리를 하던가 위 코드처럼 nickname으로 대신 할 수 있도록 하는 처리가 필요합니다.<br>
이 부분은 원하는대로 정보를 입력받아 처리해주세요.

 저는 실명이 꼭 필요한 것이 아니었기에 nickname으로 대체할 수 있도록 처리하였습니다.<br>
또 출력해보면 알겠지만 access_token값은 매우 깁니다. 그래서 저는 토큰값의 일부를 뚝 잘라 이용하였습니다.<br>
물론 그대로 이용하는게 가장 좋습니다만, 저는 공부용이었기 때문에 잘라서 이용했습니다.<br>
``` java
//예시
String splitToken = access_token.substring(1, 3) + access_token.substring(40, 43);
```
위의 예시 코드는 token값을 두 번 잘라내고 그걸 붙여 사용하는 식으로 작성한 것입니다.
원하는 바에 따라 세 번을 substring해서 붙이든 아니면 한 번만 substring해서 잘라내든 token값을 이용하시면 됩니다.
토큰을 관리법은 가장 좋다고 생각되는 방식으로 관리되게끔 구성하시면 되겠습니다.

JSP-SERVLET을 이용했으므로 모든 코드를 작성 한 후에 requestDispatcher로 원하는 view에 forward시키면 비로소 연동은 끝입니다.

### 결과

![""](https://jeongdoc.com/assets/img/2019/05/naverloginapi3-2.png)

![""](https://jeongdoc.com/assets/img/2019/05/naverloginapi3-3.png)

![""](https://jeongdoc.com/assets/img/2019/05/naverloginapi3-4.png)

 제가 만든 예시를 가져와봤습니다.<br>
간단하게 네아로로 얻은 사용자 정보 데이터를 forward시켜서 페이지에 노출되게 하였습니다.<br>
잘 적용했다면 jsp view파일에서 원하는 정보를 보이게 하는건 그다지 어려운 일은 아닙니다.

***

 네이버아이디로로그인 API 적용법을 총 3편에 걸쳐 작성해보았습니다.<br>
기록, 복습용으로 적기 시작한 것인데 실제로 도움이 많이 되었습니다.<br>
적용한다고 고생 좀 했었는데 막상 작동하는거보면 뿌듯하기도 했었고... 기억이 새록새록하네요.<br>

네아로 API연동을 Servlet을 이용하여 구성하고자 하시는 분들께 작게나마 도움이 되었으면 좋겠습니다.
