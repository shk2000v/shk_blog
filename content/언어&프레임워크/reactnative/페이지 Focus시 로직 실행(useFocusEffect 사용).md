---
title: 페이지 Focus시 로직 실행(useFocusEffect 사용)
tags:
  - reactnative
draft: false
date: 2025-05-03 00:14
---

- useFocusEffect로 화면을 볼때 이벤트를 발생시키는 Hooks
- 참조 : [https://reactnavigation.org/docs/use-focus-effect/](https://reactnavigation.org/docs/use-focus-effect/)
- 예시)
  ```jsx
  useFocusEffect(
      React.useCallback(() => {
        let isActive = true;

        const fetchInquiry = async () => {
          let dataList: InquiryType<string>[] = [];
          firestore()
            .collection('Inquiry')
            .where('uid', '==', user.uid)
            .onSnapshot(querySnapshot => {
              if (isActive) {
                if (querySnapshot.empty) {
                  setInquiryList([]);
                } else {
                  querySnapshot.forEach(e => {
                    dataList.push(e.data() as InquiryType<string>);
                  });
                }
              }
            });
          setInquiryList(dataList);
        };

        fetchInquiry();

        return () => {
          isActive = false;
        };
      }, [user]),
    );
  ```
