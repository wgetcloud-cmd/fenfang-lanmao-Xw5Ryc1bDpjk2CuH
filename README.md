
![](https://img2024.cnblogs.com/blog/468667/202412/468667-20241215132718443-632146843.gif)
 


【引言】


在鸿蒙NEXT平台上，我们可以轻松地开发出一个经纬度距离计算器，帮助用户快速计算两点之间的距离。本文将详细介绍如何在鸿蒙NEXT中实现这一功能，通过简单的用户界面和高效的计算逻辑，为用户提供便捷的服务。


【环境准备】


• 操作系统：Windows 10


• 开发工具：DevEco Studio NEXT Beta1 Build Version: 5\.0\.3\.806


• 目标设备：华为Mate60 Pro


• 开发语言：ArkTS


• 框架：ArkUI


• API版本：API 12


【思路】


在本案例中，我们将创建一个名为“距离计算器”的组件，用户可以输入起点和终点的经纬度，系统将自动计算并显示两点之间的距离。以下是实现的主要思路：


1 组件结构设计：


使用Column和Row布局组件来组织界面元素，使其具有良好的可读性和用户体验。在界面顶部添加标题，明确应用的功能。


2 输入区域：


提供两个输入框，分别用于输入起点和终点的经纬度。用户可以手动输入，也可以通过点击示例按钮快速填充常用位置（如北京和上海）。设计清空按钮，方便用户快速重置输入。


3 状态管理：


使用@State装饰器管理组件的状态，包括输入框的聚焦状态、经纬度值和计算结果。通过@Watch装饰器监视输入变化，确保在用户输入经纬度时，能够实时更新计算结果。


4 距离计算逻辑：在输入变化时，调用地图模块的calculateDistance方法，计算两点之间的距离，并将结果更新到界面上。结果以公里为单位显示，确保用户能够直观理解计算结果。


5 界面美化：通过设置颜色、边框、圆角等样式，使界面更加美观和用户友好。使用适当的字体和大小，确保信息的清晰可读。


【完整代码】



[?](https://github.com)

| 123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193 | `import` `{ mapCommon } from` `'@kit.MapKit'``;` `// 导入地图通用模块``import` `{ map } from` `'@kit.MapKit'``;` `// 导入地图模块` `@Entry` `// 入口装饰器，标识该组件为应用的入口``@Component` `// 组件装饰器，定义一个组件``struct DistanceCalculator {` `// 定义一个名为 DistanceCalculator 的结构体``@State` `private` `primaryColor: string =` `'#fea024'``;` `// 定义主题颜色，初始值为橙色``@State` `private` `fontColor: string =` `"#2e2e2e"``;` `// 定义字体颜色，初始值为深灰色``@State` `private` `isStartFocused: boolean =` `false``;` `// 定义起点输入框的聚焦状态，初始为 false``@State` `private` `isEndFocused: boolean =` `false``;` `// 定义终点输入框的聚焦状态，初始为 false``@State` `private` `isSecondStartFocused: boolean =` `false``;` `// 定义第二起点输入框的聚焦状态，初始为 false``@State` `private` `isSecondEndFocused: boolean =` `false``;` `// 定义第二终点输入框的聚焦状态，初始为 false``@State` `private` `baseSpacing: number = 30;` `// 定义基础间距，初始值为 30``@State @Watch(``'onInputChange'``)` `private` `startLongitude: string =` `""``;` `// 定义起点经度，初始为空，并监视输入变化``@State @Watch(``'onInputChange'``)` `private` `startLatitude: string =` `""``;` `// 定义起点纬度，初始为空，并监视输入变化``@State @Watch(``'onInputChange'``)` `private` `endLongitude: string =` `""``;` `// 定义终点经度，初始为空，并监视输入变化``@State @Watch(``'onInputChange'``)` `private` `endLatitude: string =` `""``;` `// 定义终点纬度，初始为空，并监视输入变化``@State distance: number = 0;` `// 定义两点之间的距离，初始值为 0` `aboutToAppear(): void {` `// 生命周期钩子函数，组件即将显示时调用``this``.onInputChange();` `// 调用输入变化处理函数以初始化``}` `onInputChange() {` `// 输入变化处理函数``let` `fromLatLng: mapCommon.LatLng = {` `// 创建起点经纬度对象``latitude: Number(``this``.startLatitude),` `// 将起点纬度转换为数字``longitude: Number(``this``.startLongitude)` `// 将起点经度转换为数字``};``let` `toLatLng: mapCommon.LatLng = {` `// 创建终点经纬度对象``latitude: Number(``this``.endLatitude),` `// 将终点纬度转换为数字``longitude: Number(``this``.endLongitude)` `// 将终点经度转换为数字``};``this``.distance = map.calculateDistance(fromLatLng, toLatLng);` `// 计算起点和终点之间的距离``}` `build() {` `// 构建界面函数``Column() {` `// 垂直布局容器``// 标题栏，展示应用名``Text(``"经纬度距离计算"``)` `// 创建文本组件，显示标题``.width(``'100%'``)` `// 设置宽度为 100%``.height(54)` `// 设置高度为 54 像素``.fontSize(18)` `// 设置字体大小为 18``.fontWeight(600)` `// 设置字体粗细为 600``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.textAlign(TextAlign.Center)` `// 设置文本对齐方式为居中``.fontColor(``this``.fontColor);` `// 设置字体颜色为定义的字体颜色` `// 输入区域``Column() {` `// 垂直布局容器``Row() {` `// 水平布局容器``Text(``'示例（北京-->上海）'``)` `// 创建文本组件，显示示例信息``.fontColor(``"#5871ce"``)` `// 设置字体颜色为蓝色``.fontSize(18)` `// 设置字体大小为 18``.padding(`${``this``.baseSpacing / 2}lpx`)` `// 设置内边距``.backgroundColor(``"#f2f1fd"``)` `// 设置背景颜色``.borderRadius(5)` `// 设置圆角半径为 5``.clickEffect({ level: ClickEffectLevel.LIGHT, scale: 0.8 })` `// 设置点击效果``.onClick(() => {` `// 点击事件处理``this``.startLongitude =` `"116.4074"``;` `// 设置起点经度为北京经度``this``.startLatitude =` `"39.9042"``;` `// 设置起点纬度为北京纬度``this``.endLongitude =` `"121.4737"``;` `// 设置终点经度为上海经度``this``.endLatitude =` `"31.2304"``;` `// 设置终点纬度为上海纬度``});``Blank();` `// 占位符，用于占据空间``Text(``'清空'``)` `// 创建文本组件，显示“清空”按钮``.fontColor(``"#e48742"``)` `// 设置字体颜色为橙色``.fontSize(18)` `// 设置字体大小为 18``.padding(`${``this``.baseSpacing / 2}lpx`)` `// 设置内边距``.clickEffect({ level: ClickEffectLevel.LIGHT, scale: 0.8 })` `// 设置点击效果``.backgroundColor(``"#ffefe6"``)` `// 设置背景颜色``.borderRadius(5)` `// 设置圆角半径为 5``.onClick(() => {` `// 点击事件处理``this``.startLongitude =` `""``;` `// 清空起点经度``this``.startLatitude =` `""``;` `// 清空起点纬度``this``.endLongitude =` `""``;` `// 清空终点经度``this``.endLatitude =` `""``;` `// 清空终点纬度``});``}.height(45)` `// 设置行高为 45 像素``.justifyContent(FlexAlign.SpaceBetween)` `// 设置子元素在主轴上的对齐方式``.width(``'100%'``);` `// 设置宽度为 100%` `Divider().margin({ top: 5, bottom: 5 });` `// 创建分隔符，设置上下边距` `// 起点输入``Row() {` `// 水平布局容器``Text(``'起点'``)` `// 创建文本组件，显示“起点”``.fontWeight(600)` `// 设置字体粗细为 600``.fontSize(18)` `// 设置字体大小为 18``.fontColor(``this``.fontColor);` `// 设置字体颜色为定义的字体颜色``}``.margin({ bottom: `${``this``.baseSpacing}lpx`, top: `${``this``.baseSpacing}lpx` });` `// 设置上下边距` `Row() {` `// 水平布局容器``TextInput({ text: $$``this``.startLongitude, placeholder:` `'经度'` `})` `// 创建起点经度输入框``.caretColor(``this``.primaryColor)` `// 设置光标颜色为主题颜色``.layoutWeight(1)` `// 设置布局权重``.type(InputType.NUMBER_DECIMAL)` `// 设置输入类型为小数``.placeholderColor(``this``.isStartFocused ?` `this``.primaryColor : Color.Gray)` `// 设置占位符颜色``.fontColor(``this``.isStartFocused ?` `this``.primaryColor :` `this``.fontColor)` `// 设置字体颜色``.borderColor(``this``.isStartFocused ?` `this``.primaryColor : Color.Gray)` `// 设置边框颜色``.borderWidth(1)` `// 设置边框宽度``.borderRadius(10)` `// 设置圆角半径为 10``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.showUnderline(``false``)` `// 不显示下划线``.onBlur(() =>` `this``.isStartFocused =` `false``)` `// 失去焦点时设置聚焦状态为 false``.onFocus(() =>` `this``.isStartFocused =` `true``);` `// 获得焦点时设置聚焦状态为 true` `Line().width(10);` `// 创建分隔符，设置宽度为 10 像素` `TextInput({ text: $$``this``.startLatitude, placeholder:` `'纬度'` `})` `// 创建起点纬度输入框``.caretColor(``this``.primaryColor)` `// 设置光标颜色为主题颜色``.layoutWeight(1)` `// 设置布局权重``.type(InputType.NUMBER_DECIMAL)` `// 设置输入类型为小数``.placeholderColor(``this``.isEndFocused ?` `this``.primaryColor : Color.Gray)` `// 设置占位符颜色``.fontColor(``this``.isEndFocused ?` `this``.primaryColor :` `this``.fontColor)` `// 设置字体颜色``.borderColor(``this``.isEndFocused ?` `this``.primaryColor : Color.Gray)` `// 设置边框颜色``.borderWidth(1)` `// 设置边框宽度``.borderRadius(10)` `// 设置圆角半径为 10``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.showUnderline(``false``)` `// 不显示下划线``.onBlur(() =>` `this``.isEndFocused =` `false``)` `// 失去焦点时设置聚焦状态为 false``.onFocus(() =>` `this``.isEndFocused =` `true``);` `// 获得焦点时设置聚焦状态为 true``}` `// 终点输入``Text(``'终点'``)` `// 创建文本组件，显示“终点”``.fontWeight(600)` `// 设置字体粗细为 600``.fontSize(18)` `// 设置字体大小为 18``.fontColor(``this``.fontColor)` `// 设置字体颜色为定义的字体颜色``.margin({ bottom: `${``this``.baseSpacing}lpx`, top: `${``this``.baseSpacing}lpx` });` `// 设置上下边距` `Row() {` `// 水平布局容器``TextInput({ text: $$``this``.endLongitude, placeholder:` `'经度'` `})` `// 创建终点经度输入框``.caretColor(``this``.primaryColor)` `// 设置光标颜色为主题颜色``.layoutWeight(1)` `// 设置布局权重``.type(InputType.NUMBER_DECIMAL)` `// 设置输入类型为小数``.placeholderColor(``this``.isSecondStartFocused ?` `this``.primaryColor : Color.Gray)` `// 设置占位符颜色``.fontColor(``this``.isSecondStartFocused ?` `this``.primaryColor :` `this``.fontColor)` `// 设置字体颜色``.borderColor(``this``.isSecondStartFocused ?` `this``.primaryColor : Color.Gray)` `// 设置边框颜色``.borderWidth(1)` `// 设置边框宽度``.borderRadius(10)` `// 设置圆角半径为 10``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.showUnderline(``false``)` `// 不显示下划线``.onBlur(() =>` `this``.isSecondStartFocused =` `false``)` `// 失去焦点时设置聚焦状态为 false``.onFocus(() =>` `this``.isSecondStartFocused =` `true``);` `// 获得焦点时设置聚焦状态为 true` `Line().width(10);` `// 创建分隔符，设置宽度为 10 像素` `TextInput({ text: $$``this``.endLatitude, placeholder:` `'纬度'` `})` `// 创建终点纬度输入框``.caretColor(``this``.primaryColor)` `// 设置光标颜色为主题颜色``.layoutWeight(1)` `// 设置布局权重``.type(InputType.NUMBER_DECIMAL)` `// 设置输入类型为小数``.placeholderColor(``this``.isSecondEndFocused ?` `this``.primaryColor : Color.Gray)` `// 设置占位符颜色``.fontColor(``this``.isSecondEndFocused ?` `this``.primaryColor :` `this``.fontColor)` `// 设置字体颜色``.borderColor(``this``.isSecondEndFocused ?` `this``.primaryColor : Color.Gray)` `// 设置边框颜色``.borderWidth(1)` `// 设置边框宽度``.borderRadius(10)` `// 设置圆角半径为 10``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.showUnderline(``false``)` `// 不显示下划线``.onBlur(() =>` `this``.isSecondEndFocused =` `false``)` `// 失去焦点时设置聚焦状态为 false``.onFocus(() =>` `this``.isSecondEndFocused =` `true``);` `// 获得焦点时设置聚焦状态为 true``}``}``.width(``'650lpx'``)` `// 设置输入区域宽度为 650 像素``.padding(`${``this``.baseSpacing}lpx`)` `// 设置内边距``.margin({ top: 20 })` `// 设置上边距为 20 像素``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.borderRadius(10)` `// 设置圆角半径为 10``.alignItems(HorizontalAlign.Start);` `// 设置子元素在交叉轴上的对齐方式` `// 显示计算结果``Column() {` `// 垂直布局容器``Text() {` `// 文本组件``Span(`两点之间的距离是：`)` `// 创建文本片段，显示提示信息``Span(`${(``this``.distance / 1000).toFixed(2)} `).fontColor(``this``.primaryColor)` `// 创建文本片段，显示距离（公里），并设置颜色``Span(`公里`)` `// 创建文本片段，显示单位“公里”``}``.fontWeight(600)` `// 设置字体粗细为 600``.fontSize(18)` `// 设置字体大小为 18``.fontColor(``this``.fontColor);` `// 设置字体颜色为定义的字体颜色``}``.width(``'650lpx'``)` `// 设置结果显示区域宽度为 650 像素``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.borderRadius(10)` `// 设置圆角半径为 10``.padding(`${``this``.baseSpacing}lpx`)` `// 设置内边距``.margin({ top: `${``this``.baseSpacing}lpx` })` `// 设置上边距``.alignItems(HorizontalAlign.Start);` `// 设置子元素在交叉轴上的对齐方式``}``.height(``'100%'``)` `// 设置整个组件高度为 100%``.width(``'100%'``)` `// 设置整个组件宽度为 100%``.backgroundColor(``"#eff0f3"``);` `// 设置背景颜色为浅灰色``}``}` |
| --- | --- |



　　


 本博客参考[milou加速器](https://jiechuangmoxing.com)。转载请注明出处！
