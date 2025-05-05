---
title: Keyboard-input 시 하단 추천탭 설정법
tags:
  - "#reactnative"
draft: true
date: 2025-05-03 00:14
---

> [!important] 문서 주의 사항 :
> 해당 문서는 다듬는 중
> 코드와 예시 화면만 참조중입니다.

### 적용된 화면 예시)

![[스크린샷, 2024-10-28 오전 8.57.08.jpeg|300]]

### 하단 추천 탭 예시 코드

```tsx
const RecoomendWordList = (onPress: (str: string) => void) => {
  return (
    <FlatList
      data={MISSION_MOCK_DATA}
      horizontal
      keyExtractor={(item, index) => index.toString()}
      showsVerticalScrollIndicator={false}
      showsHorizontalScrollIndicator={false}
      pt={"15px"}
      pb={"3px"}
      borderTopColor={"#E9E6E6"}
      borderTopWidth={"0.5px"}
      // 하단 컴포넌트 클릭시 키보드가 내려가기 방지
      keyboardShouldPersistTaps={"always"}
      renderItem={({ index, item }) => (
        <>
          <MissionChip onPress={() => onPress(item)} ml={index === 0 ? "16px" : 0} mr={"8px"}>
            {item}
          </MissionChip>
        </>
      )}
    />
  )
}
```

### props로 useForm의 요소를 받고 input이 focus된 index를 찾는 함수가 포함된 useHooks

```tsx
import { useState } from "react"
import {
  FieldValues,
  Path,
  PathValue,
  UseFormGetValues,
  UseFormSetValue,
  UseFormTrigger,
} from "react-hook-form"
import { NativeSyntheticEvent, TextInputSelectionChangeEventData } from "react-native"

type useInputContorlHooksProps<T extends FieldValues = FieldValues> = {
  getValues: UseFormGetValues<T>
  setValue: UseFormSetValue<T>
  trigger: UseFormTrigger<T>
}
const useInputContorlHooks = <T extends FieldValues = FieldValues>({
  getValues,
  setValue,
  trigger,
}: useInputContorlHooksProps<T>) => {
  const [showRecommendation, setShowRecommendation] = useState<keyof T | "">("")
  const [cursorPosition, setCursorPosition] = useState(0)
  const onFocusFn = (str: keyof T) => () => {
    setShowRecommendation(str)
  }
  const onBlurFn = () => {
    setShowRecommendation("")
  }
  const onPressAddRecommendation = (str: string) => {
    if (showRecommendation === "") return
    const target = showRecommendation as Path<T>
    const value = getValues(target) as string
    // input의 value가 없을 경우 작동되고 void return
    if (!value) {
      // 타입에러 방지를 위한 PathValue 타입으로 캐스팅
      setValue(target, str as PathValue<T, Path<T>>)
      trigger(target)
      return
    }
    const newValue = value.slice(0, cursorPosition) + str + value.slice(cursorPosition)
    // 타입에러 방지를 위한 PathValue 타입으로 캐스팅
    setValue(target, newValue as PathValue<T, Path<T>>)
    trigger(target)
  }
  const handleSelectionChange = (
    event: NativeSyntheticEvent<TextInputSelectionChangeEventData>,
  ) => {
    // console.log(event.nativeEvent.selection.start);
    setCursorPosition(event.nativeEvent.selection.start)
  }
  return {
    showRecommendation,
    onFocusFn,
    onBlurFn,
    onPressAddRecommendation,
    handleSelectionChange,
  }
}

export default useInputContorlHooks
```
