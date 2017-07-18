## ReactNative原生组件

## props
- 属性
- 在组件上面自定义到属性我们可以通过`props`访问到, `this.props.name`
- 父组件可以通过属性给子组件传递数据

## state
- 状态
- 我们可以通过状态到变化来修改显示状态

```js
// 定义状态
this.state = {
    display: true
}
// 修改状态
this.setState(a => {
    return {
        display: !a.display
    }
})
```


## AppRegistry
- 用于注册一个app

```js
export default 类名
AppRegistry.registerComponent('testRN',() => 类名)
```

## StyleSheet
- 用于创建一个样式

```js
const styles = StyleSheet.create({
    container: {
        color: '#0094ff'
    }
})
```

## Text
- 用于显示一个标签

```js
<Text style={styles.container}>要显示到文本</Text>
```

## View
- 用于显示一个视图

```js
<View name='自定义属性'>要包含的组件</View>
```

## TextInput
- 输入框

```jsx
// _test是我们自定义到一个函数
<TextInput placeholder='提示文本' style={{height: 30}} onChangeText={this._test}/>
```

## Button
- 顾名思义就是按钮

```js
<Button onPress={() => {Alert.alert('this is button press')}} />
```

## Alert
- 调用原生到alert
```js
Alert.alert('msg')
```

## TouchableHighlight
- 这也是一个类似按钮组件,里面可以嵌套其他组件,不过这个可以设置点击时候到背景颜色
- 通过设置`underlayColor`属性来设置激活到时候到背景颜色

```js
<TouchableHighlight onPress={this._onPressTest} underlayColor='red'>
    <View>
        <Text>TouchableHighlight</Text>
    </View>
</TouchableHighlight>
```

## TouchableOpacity
- 同上, 里面可以嵌套其他组件, 再激活到时候会有透明度的变化
- 不用设置任何属性

```js
<TouchableOpacity onPress={this._onPressTest}>
    <View>
        <Text>TouchableOpacity</Text>
    </View>
</TouchableOpacity>
```

## TouchableNativeFeedback
- 此组件可以让点击效果出现再android上出现水滴效果, 不支持ios

```js
<TouchableNativeFeedback>
    <View>
        <Text>TouchableNativeFeedback</Text>
    </View>
</TouchableNativeFeedback>
```
## TouchableWithoutFeedback
- 此组件没有任何反应
- 用法同上

## ScrollView
- 可以滑动到组件,`View`是不可以滑动到组件
- `horizontal` 设置水平滑动
- `pagingEnabled`启用分页滑动
- `maximumZoomScale` 设置使用双指放大页面
- `minimumZoomScale` 通过手势缩小页面

```js
<ScrollView>
    <Text style={{fontSize: 100}}>Test ScrollView</Text>
    <Text style={{fontSize: 100}}>Test ScrollView</Text>
    <Text style={{fontSize: 100}}>Test ScrollView</Text>
<ScrollView>
```

## FlatList
- 列表组件
- `data` 用于展示到数据
- `renderItem` 用于在FlatList中渲染的组件

```js
<View>
    <FlatList data={[
        {key:'one'},
        {key:'two'},
        {key:'three'},
        {key:'four'},
        {key:'five'}
    ]} renderItem={(item) => <Text>{item.key}</Text>} />
</View>
```

## SectionList 
- 实现iOS中UITableView的效果
- `sections` 设置要展示到数据, 包括头和体
- `renderItem` 设置要展示到内容体
- `renderSectionHeader` 设置要展示的内容头

```js
<View>
    <SectionList 
        sections={[
            {title:}
        ]}
    />
</View>
```



