---
title: Jekyll 폰트 적용
date: '2022-10-27 10:35:00 +0900'
categories: [JEKYLL,FONT]
tags: [jekyll,font]
---

### 들어가며

대부분의 jekyll테마에 기본 설정되어있는 폰트들은 한글이 그리 예쁘게(?) 표현되지 않습니다.    
따라서 원하는 폰트 사용을 위한 설정 변경 방법을 간단하게 소개하고자 합니다.

### 1. 기본 폰트 설정 확인
 블로그 이전을 시작하며 jekyll 테마를 이용했습니다.    
대개 공통적으로 적용되는 UI관련 스타일시트는 main.scss, common.scss와 같은 이름의 파일에 작성되어 있습니다.    
물론 어떤 테마를 사용하였느냐에 따라 구성은 조금씩 다를 수 있습니다.

우선 폰트 설정이 작성되어있는 부분을 확인합니다.
공통 적용 css(scss) 파일을 열어보면 아래처럼 body에 적용되어있음을 확인할 수 있습니다.
다른 테마는 어떤지 모르겠습니다만, 공통 폰트 적용은 body에 되어있는게 대부분일 것이라 생각하긴 합니다.

``` css
body {
  background: var(--body-bg);
  color: var(--text-color);
  letter-spacing: 0;
  -webkit-font-smoothing: antialiased;
  /* font-family 속성으로 폰트들이 적용되어 있음 */
  font-family: "NotoSans", "Roboto";
  line-height: 1.75;
}
```
font-family에 적혀있는 순서대로 폰트가 적용됩니다.    
> NotoSans 먼저 적용, 이 폰트를 찾지 못할 경우 Roboto 적용
따라서 원하는 폰트 적용을 위해서는 맨 앞쪽에 해당 폰트 명칭을 작성해야합니다.    
혹은 그 폰트 명칭만 남겨두거나요.

### 2. 폰트 적용
 원하는 폰트를 찾았다면 폰트의 경로를 css(scss) 파일에 작성해야합니다.    
저는 직접 다운받아 해당 경로까지를 입력하였습니다.    
만약 구글 폰트를 골랐다면 제공하는 import 구문이 있을텐데 그걸 그대로 복붙하면 됩니다.

import 구문은 파일의 최상단에 작성해주세요.

``` css
/* 구글 import 예시 */
@import url('https://fonts.googleapis.com/css2?family=Nanum+Gothic:wght@400;800&family=Roboto:wght@100&display=swap');

/* 내가 입력한 import url 예시 */
@font-face {
  font-family: "Roboto";
  src: url('{폰트가 있는 폴더 경로}/Roboto.eot') format('eot');
  src: url('{폰트가 있는 폴더 경로}/Roboto.woff') format('woff'); 
}

/* import한 폰트 적용 */
body {
  background: var(--body-bg);
  color: var(--text-color);
  letter-spacing: 0;
  -webkit-font-smoothing: antialiased;
  /* 나눔고딕 적용 */
  font-family: "Nanum Gothic", "NotoSans", "Roboto";
  line-height: 1.75;
}
```

 직접 다운받아 적용할 경우 경로를 잘못 기입하진 않았는지 확인합니다.    
설정을 제대로 했는데 변경 내역이 바로 반영되지 않을 경우 브라우저의 캐시 삭제를 해주세요.

모든 과정을 마치셨다면 변경한 폰트로 서체가 바뀌었음을 확인할 수 있습니다.
