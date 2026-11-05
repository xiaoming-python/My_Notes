### 标签
#### 表单标签 `<form>`
- 被表单标签包裹的内容表示一个表单，例如：`<form >...</form>`
- 表单头部基本要加入参数，action是表示表单数据传到哪个主机地址，method表示请求方式，例如：`<form action="www.myweb/1.php" method="POST">...</form>` 表示这个表单传到`www.my....`,用`post`方式请求
##### 输入标签`<input>`
- 输入标签通常与表单一起使用，包裹在表单标签中，表示输入内容传给当前表单
- 输入标签有多钟类型，用`type` 表示类型，`name`表示输入值的名字，value表示默认值，例如：`<form ...><input type='text' name='userinput' value='none'></form>`
###### 文本类型`type='text'`
- 接受用户输入的文本信息
###### 密码类型 `type='passwoed'`
- 接受用户输入的密码，输入后会显示成星号`*`
###### 提交类型 `type='submit'`
- 变成按钮属性，点击后提交表单，`vlaue`为按钮显示的文字
###### 单选框 `type='radio'`

- **说明**：允许用户从一组选项中**只能选择一个**选项（具有互斥性）。
- **关键属性**：
    - **`name`（至关重要）**：同一组单选框**必须拥有完全相同的 `name` 属性值**。只有这样，浏览器才会认为它们是同一组的，从而实现“互斥”（选了一个，另一个自动取消）。
    - **`value`**：提交给服务器的实际值。
    - **`checked`**：默认选中该选项。
    - **`id`** 和 `<label>`：通常配合 `<label for="id名">` 使用，这样用户点击文字也能选中，体验更好（也方便自动化测试工具定位）。
        

**代码示例**：
```html
<p>请选择你的性别：</p>
<input type="radio" id="male" name="gender" value="male" checked>
<label for="male">男</label>
<input type="radio" id="female" name="gender" value="female">
<label for="female">女</label>
```
**效果:**
<p>请选择你的性别：</p>
<input type="radio" id="male" name="gender" value="male" checked>
<label for="male">男</label>
<input type="radio" id="female" name="gender" value="female">
<label for="female">女</label>

###### 复选框类型 `type='checkbox'`

 - **说明**：允许用户从一组选项中**选择零个、一个或多个**选项。
- **关键属性**：
    - `name`：提交给服务器的键名。如果是一组复选框，通常会将 `name` 设置为数组形式（如 `name="hobby[]"`），以便后端接收多个值。
    - `value`：该选项实际提交的值。如果不写，默认提交 `on`。
    - `checked`：默认选中该复选框。

**代码示例**：
```html
<p>你的爱好：</p>
<input type="checkbox" name="hobby[]" value="reading" checked> 阅读
<input type="checkbox" name="hobby[]" value="coding"> 编程
<input type="checkbox" name="hobby[]" value="gaming"> 打游戏
```
**效果:**
<p>你的爱好：</p>
<input type="checkbox" name="hobby[]" value="reading" checked> 阅读
<input type="checkbox" name="hobby[]" value="coding"> 编程
<input type="checkbox" name="hobby[]" value="gaming"> 打游戏

###### 文件上传类型`type='file'`

1. 最基本的代码

```html
<form action="upload.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="myfile">
    <input type="submit" value="上传文件">
</form>
```
🚨 2. 绝对不可省略的属性：`enctype`

很多人写了 `type="file"` 却上传失败，99% 是因为漏掉了 `<form>` 标签里的 `enctype="multipart/form-data"`。

- 普通的表单默认是 `application/x-www-form-urlencoded`，它只能传文本（比如账号密码）。
    
- 要传文件（二进制数据），**必须**加上 `enctype="multipart/form-data"`，且提交方式必须是 `method="POST"`。
#### 图片标签`<img>`
- `img`是显示图片的标签
- `<img` 开始，`>`结束
-  `src`表示图片的地址
-  `onerror` 错误时执行
 例如：
 `<img src=22 onerror=alert(1)>` 找不到22，就会执行alert(1)

#### 下拉列表 (`Dropdown List`)

- **标签**：`<select>` 和 `<option>` 组合使用。
- **说明**：提供一个折叠的选项列表，用户从中选择（默认单选，加 `multiple` 属性可多选）。
- **关键属性**：
    - `<select>` 用于定义下拉框的整体，需要指定 `name`。
    - `<option>` 用于定义里面的每一个选项，包含 `value` 属性。
    - `selected` 属性加在 `<option>` 上，表示默认选中。

**代码示例**：

```html
<select name="city">
    <option value="bj">北京</option>
    <option value="sh" selected>上海</option>
    <option value="gz">广州</option>
</select>
```
**效果:**

<select name="city">
    <option value="bj">北京</option>
    <option value="sh" selected>上海</option>
    <option value="gz">广州</option>
</select>

#### 多行文本 (Multi-line Text)

- **标签**：`<textarea>`
- **说明**：用于让用户输入长篇幅的文本（如留言、评论、个人简介）。它是一个**双标签**，输入的内容直接写在开标签和闭标签之间。
- **关键属性**：
    - `name`：提交给服务器的键名。
    - `rows`：文本框显示的行数（高度）。
    - `cols`：文本框显示的列数（宽度）。
    - _注意_：`<textarea>` **没有** `value` 属性，它的初始值直接写在两个标签中间。

**代码示例**：

```html
<textarea name="message" rows="5" cols="30">这里写默认文本...</textarea>
```
**效果:**
<textarea name="message" rows="5" cols="30">这里写默认文本...</textarea>