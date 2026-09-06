# 一、id选择器定位
   - 查找id为one的元素
```python
   ele1=page.ele("#one")
```
   -  和上面一行一致  
```python
    ele2 = page.ele('#=one')
```
   - 查找id属性包含ne的元素
```python
   ele3 = page.ele('#:ne')
```  
  - 查找id属性以on开头的元素
```python
   ele4 = page.ele('#^on')
```
  - 查找id属性以ne结尾的元素
```python
   ele5 = page.ele('#$ne')
```
# 二、class选择器定位
  - 查找class属性为p_cls的元素
  ```python
  e1 = page.ele('.p_cls')
  ```
  - 与上一行一致
  ```python
  e2 = page.ele('.=p_cls')
  ```
  -  查找class属性包含_cls的元素
  ```python
  e3 = page.ele('.:_cls')
  ```
  - 查找class属性以p_开头的元素
  ```python
  e4 = page.ele('.^p_')
  ```
  - 查找class属性以_cls结尾的元素
```python
  e5 = page.ele('.$_cls')
```
# 三、文本选择器定位
   - 查找文本为“第二行”的元素
```python
  element1 = page.ele('text=第二行')
```
  - 查找文本包含“第二”的元素
```python
  element2 = page.ele('text:第二')
```
   - 与上一行一致
```python
  element3 = page.ele('第二')
```
  
# 四、标签选择器定位
- 查找第一个div元素
```python
ele1 = page.ele('tag:div') 
```
 - 与单属性查找配合使用
```python
ele2 = page.ele('tag:p@class=p_cls')
```
- 与多属性查找配合使用
```python
ele3 = page.ele('tag:p@@class=p_cls@@text()=第二行')
```

# 五、xpath定位
```python
eles = page.eles('xpath://div/p[@id="row1"]')e5 = page.ele('.$_cls')
```
