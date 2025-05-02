### 상황 :

- 프로필 이미지를 바꾸는 Post요청 등으로 인해 이미지가 변경 되어야 하는데 해당 변경되는 uri가 동일할 경우 참조 uri의 내용이 달라져도 해당 주소로 이미지가 캐싱되어 이미지 변경이 이루어지지 않는다

### 해결 :

- 이미지의 Uri 주소 뒤에 garbage값을 붙여 timestamp 등의 값을 붙여 리랜더링 될때마다 다른값을 적용시켜 uri가 캐싱되어 같은값을 불러오는것을 방지

### code :

```tsx
const Components = ({ image }: { image: String }) => {
  const timeStamp = new Date().getTime()
  // before
  // const uri = image;
  // after
  const uri = `${image}?${timeStamp}`

  return (
    <View>
      <FastImage resizeMode={"contain"} style={[]} source={uri} />
    </View>
  )
}
```
