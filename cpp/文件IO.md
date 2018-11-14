
# 文件IO

进行文件操作，需要在源文件中包含头文件`<iostream>`和`<fstream>`。  

`<fstream>`中定义了三个新的数据类型：  

| 类型 | 描述 |
| --- | --- |
| ofstream | 输出文件流，用于创建文件并向文件写入信息 |
| ifstream | 输入文件流，用于从文件读取信息 |
| fstream | 文件流，同时具有`ofstream`和`ifstream`两种功能，可以创建文件、向文件写入信息，从文件读取信息 |

## 打开

3个类型都可以打开文件，其中`ofstream`和`fstream`可以进行写操作，`ifstream`只能进行读操作。  

下面是`open()`函数的标准语法，该函数是`ofstream`、`ifstream`和`fstream`对象的一个成员。  

```
void open(const char *filename[, ios::openmode mode]);
```
在这里，`open()`成员函数的第一个参数指定要打开的文件的名称和位置，第二个参数定义文件被打开的模式。

| 模式 | 描述 |
| --- | --- |
| ios::app | 追加模式。所有写入都追加到文件末尾 |
| ios::ate | 文件打开后定位到文件末尾 |
| ios::in  | 打开文件用于读取 |
| ios::out | 打开文件用于写入 |
| ios::trunc | 如果文件已经存在，其内容将在打开文件之前被截断，即把文件长度设为0 |

上面的模式可以结合使用，不同模式间用`|`连接。

```
ofstream outfile;
outfile.open('file.dat', ios::out | ios::trunc);

fstream afile;
afile.open("file.dat", ios::out | ios::in);
```

`ofstream`打开文件，默认情况下，模式为写模式，如果文件不存在，创建文件，如果文件已存在，覆盖原有文件。  
`ifstream`打开文件，默认情况下，模式为读模式。

## 关闭

程序终止前，会自动关闭刷新所有流，释放所有分配的内存，并关闭所有打开的文件。但程序员要养成习惯，在程序终止前关闭所有打开的文件。  

`close()`函数是3个对象的一个成员。  

```
void close();
```

## 读

使用流提取运算符读取信息，就像使用该运算符从键盘输入信息一样。唯一不同的是，这里使用的是`ifstream`和`fstream`对象，而不是`cin`对象。  

```
infile >> data;
```

再次读取时，会自动换到下一行读取

## 写

使用流插入运算符 `<<` 向文件写入信息，就像使用该运算符输出信息到屏幕一样。唯一不同的是，这里使用的是`ofstream`和`fstream`对象，而不是`cout`对象。  

```
outfile << data << endl;
```

读取用户输入时，使用字符数组`char data[100];`存储用户输入字符。  
读取一行输入时，使用`cin.getline(data, n);`。  
读取输入数字时，使用`cin >> data; cin.ignore();`。`ignore()`用于清空输入缓冲区，忽略掉之前读语句留下的多余字符。  

## 定位

`istream`和`ostream`都提供了重新定位文件位置指针的成员函数，分别为`seekg()`(seek get)和`seekp()`(seek put)。  

`seekg`和`seekp`有两个参数。第一个参数是长整型，表明位置指针移动的字节数。第二个参数表明位置指针移动的起始位置及移动方向：默认为`ios::beg`，从文件开头开始定位，向右移动指针；`ios::cur`，从流当前位置开始定位，向右移动指针；`ios::end`，从文件末尾开始定位，向左移动指针。  

文件位置指针是一个整数值，指定了从文件起始位置到指针所在位置的字节数。  

```
// 把文件的读指针从file开头向右移动 n 个字节
file.seekg( n );
// 把文件的读指针从file当前位置向后移动 n 个字节
file.seekg( n, ios::cur );
// 把文件的读指针从file末尾往回移动 n 个字节
file.seekg( n, ios::end );
```

## 逐行读入

```
#include <iostream>
#include <fstream>

int main()
{
    ifstream in;
    in.open("a.txt");
    string line;
    
    while (!in.eof())
    {
        getline(in, line);
    }
    
    in.close();
    
    return 0;
}
```
