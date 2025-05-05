---
title: Tmap 세팅&사용법
tags:
  - react
  - reactnative
  - reactnative/lib
draft: false
date: 2025-05-03 00:14
---

설치 패키지

```bash
$ yarn add react-native-invoke-tmap
```

Tmap에서 해주어야 할 설정

1. Tmap 에서 로그인을 해주고 계정이 없다면 회원가입 해주세요
2. Tmap api를 사용할 앱을 만들어야 합니다. 처음엔 앱이 없으니 대시보드를 통해 앱을 만들어 주세요.
3. ![[tmap_example.png]]
4. 앱 상품 사용 신청을 신청해야합니다. 무료플랜이나 그 이상을 선택하면 됩니다.

   [SK open API](https://openapi.sk.com/products/detail?linkMenuSeq=127#%EC%83%81%ED%92%88%EC%82%AC%EC%9A%A9%EC%8B%A0%EC%B2%AD%ED%95%98%EA%B8%B0)

5. 무료플랜이 적용되어있다면 App Key를 기억하면 됩니다.

앱내의 설정

- ./android/app/src/main/AndroidManifest.xml 수정
  ```xml
  <manifest xmlns:android="<http://schemas.android.com/apk/res/android>" package="com.grabber.grabbee">
      ...

      <queries>
        <package android:name="com.skt.skaf.l001mtm091" />
        <package android:name="com.skt.tmap.ku" />
      </queries>
      <application
  			...
  	   >
        <meta-data
          android:name="com.skt.tmap"
          android:value="여기에 Tmap App Key를 넣으세요" />
      </application>
  </manifest>
  ```

사용 예시)

```tsx
import {
  checkTmapApplicationInstalled,
  invokeTMap,
  complexInvokeTMap,
  InvokeTMapRouteInfo,
} from 'react-native-invoke-tmap';

// ...

//data structure
const routeInfo: InvokeTMapRouteInfo = {
  rGoName: '강남역', // 목적지명칭(필수)
  rGoX: 127.027621, // 목적지경도(필수)
  rGoY: 37.497942, // 목적지위도(필수)
  rStName: '서울역', // 출발지명칭
  rStX: 126.972646, // 출발지경도
  rStY: 37.553017, // 출발지위도
  rV1Name: '용산역', // 경유지1명칭
  rV1X: 126.964775, // 경유지1 경도
  rV1Y: 37.52989, // 경유지1 위도
  rV2Name: '사당역', // 경유지2명칭
  rV2X: 126.981633, // 경유지2경도
  rV2Y: 37.476559, // 경유지2위도
};

//in react native components
export default function App() {
    //...
		// 디바이스에 Tmap이 깔려있나 안깔려있나 확인
    const onPressCheckTMapExistButton = () => {
        checkTmapApplicationInstalled()
        .then((isExist) => {
            if (isExist) {
            setIsTMapExist(true);
            } else {
            setIsTMapExist(false);
            }
        })
        .catch(() => {
            setIsTMapExist(false);
        });
    };
		// 지정된 설정으로 티맵 실행
    const onPressInvokeTMap = () => {
        invokeTMap(routeInfo);
    };
		// 지정된 설정으로 티맵 실행
		// 만일 티맵이 설치가 안되어있다면 스토어로 이동
    const onPressComplexInvokeTMap = () => {
        complexInvokeTMap(routeInfo);
    };

    return (
        //...
    )
}
```
