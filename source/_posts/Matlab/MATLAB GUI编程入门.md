

## 1 题外话
这学期刚好在学信息论，里面各种熵函数信道容量的计算十分繁杂，于是乎想编写一个有GUI界面的信息论计算器。由于MATLAB对于矩阵的运算十分方便，所以选择以MATLAB为基础进行程序编写和界面设计。

---

## 2 回调函数callback中常用函数
1. get(hObject,'String')
- 获得一个控件的字符串属性，常配合str2double函数使用，用于获得输入的数字以及判断输入的是否为数字
- hObject相当于当前操作的控件的句柄，如果是在同一个GUI界面，可以使用`handles.Tags`，也就是 当前GUI的句柄名.要获得数据的控件标签名 来从其他控件获得数据。
2. set(hObject,'String','content')
设置一个控件的字符串属性，常用于设置输入数字的设置
3. guidata(hObject,handles)保存前面对hObject和handles的设置，一般回传函数callback的最后一行都是这段代码，用来更新数据
4. 

常用对话框
===================================
tips:
- 每个GUI窗口都有一个初始化函数，类似于`function simple_gui_OpeningFcn(hObject, eventdata, handles, varargin)`,这个初始化函数用于在创建这个GUI时创建一些参数以及在退出时返回一些参数。在这个函数中创建的参数可以被这个GUI里面的所有Object控件的回调函数调用。 
- fdsf 
=================================
常用控件

1. 弹出菜单（Pop-up Menu）
在属性设置菜单（Inspector）中的'String'项可以对弹出菜单的项目进行设置，在'String'项第一行的内容将默认显示在弹出菜单中。在回调函数中，需要对弹出菜单中的每一项设置相应的代码，常用的语句是switch...case...end语句。常用的代码段：
``` matlab
string = get(hObject,'String');
value = get(hObject,'Value');
switch string{value}
case 'A' %列表第一项
end

value = get(hObject,'Value');
switch value
case 1  %1即为列表中的第一项
end
```


=======================================
创建新窗口的方法：
1. 使用guide创建一个新窗口并对其中的控件进行设置，将其命名（假设为child.fig）
2. 在触发显示新窗口的回调函数中加上如下语句：
``` matlab
run('child');  %or open('child.fig');

h = guihandles;  %h为新窗口的句柄变量，相当于原窗口的handles，可以使用如h.text1来操作新窗口的控件
set(h.text1,'String','Hello child');  %这是一个将新窗口中的text1控件显示的字符串设置成'Hello child'的例子

```


======================================
在回调函数之间和UI之间传递数据的方法
1.setappdata(obj,name,val)

- obj为想要存储的数据所在的窗口的句柄变量，类似于handles，称为图形对象
- name为存储该数据的变量名
- val为数据的值
例如：`setappdata(f,'todaysdate',val); %将句柄名为f的UI窗口中的数据val存放在名为todaysdate的变量中`

2. val = getappdata(obj,name)
   vals = getappdata(obj)

- obj为想要取出的数据所在的窗口的句柄变量，类似于handles，它的值应该和对应的getappdata函数中的obj相同
- name为存储该数据的变量名
- 注意左边需要设置一个val变量用于存放取出该数据的变量
- vals为取出同一个图形对象中存放的所有共享数据

=========================================
在同一个窗口中根据不同选择显示不同控件的方法
初始化时将要显示的控件的Visible属性选择为On，其他控件选择为Off，在每个按钮的回调函数中利用`set(handles.edit4,'Visible','on')`等代码进行调整即可


=======================================
设置按下按钮或右上角弹出是否关闭的对话框的方法：
在按钮的回调函数或当前窗口的CloseRequestFcn函数中加上
selection = questdlg('Close the figure window?',...
    'Confirmation',...
    'Yes','No','Yes');
switch selection,
    case 'Yes',
        delete(gcf);  %gcf为当前图像句柄
    case 'No'
        return
end


============================================
matlab GUI中插入图片的方法
axes(handles.axes2);
imshow('jin.jpg');

清除图片的方法
cla reset;

不显示坐标轴的方法：
set(handles.axes1,'visible','off')
需要使用时改成on即可

=================================
控件大小通过更改position中的height和weight选项即可


换行可以使用sprintf('\n')或 [char(10,13)']实现


matlab两个浮点数或整数和浮点数比较应该用abs(a - b) < eps，而不是a - b

==================================
调整当前窗口到屏幕中央
`movegui( gcf, 'center')`。其他位置查阅movegui函数即可



===================================
matlab 显示矩阵
set(handles.uitable1, 'Data', comparison);

=================================
判断一个矩阵中所有元素是否等于0
`any(abs(input2_2_temp(:) - 0 ) > eps)`


MATLAB解线性方程组
Ax = b
x = A\b



====================================
matlab中取出一个数组中某些特定元素
***a(a == 0) %取出a中所有等于0的元素，可接赋值语句，如a(a == 0) = 1;***

=================
MATLAB 函数句柄
格式：H = @(x)cos(x)
相当于H(x) = cos(x)

==================================
标记图像中的最大值
``` matlab
%first solution
clc;clear all
x=0:0.01:2*pi;
y=sin(x);
p=find(y == max(y));
plot(x,y,'r','linewidth',2)
grid on
text(x(p),y(p),'o','color','g')
axis([0 2*pi -1.4 1.4]);

%second solution
clc;clear all
x=0:0.01:2*pi;
y=sin(x);
p=find(y==max(y));
plot(x,y,'r','linewidth',2)
grid on
axis([0 2*pi -1.4 1.4])
text(x(p),y(p),['(',num2str(x(p)),',',num2str(y(p)),')'],'color','b');
```

====================================
MATLAB GUI 设置每个控件的显示顺序
右键单击控件，有两个选项：Bring to front 和send
 to back，可以设置控件图层顺序