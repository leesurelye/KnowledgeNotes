<center><h1>文件IO操作</h1></center>



# FileInfo

文件的信息包括文件名，文件大小，修改权限，修改时间等。

> FileInfo interface

Go语言系统文件信息接口属性定义如下：

```go
type FileInfo interface {
	Name() string       // base name of the file
	Size() int64        // length in bytes for regular files; system-dependent for others
	Mode() FileMode     // file mode bits
	ModTime() time.Time // modification time
	IsDir() bool        // abbreviation for Mode().IsDir()
  Sys() interface{}   // underlying data source (can return nil)
}
```

> fileStat结构体

```go
// A fileStat is the implementation of FileInfo returned by Stat and Lstat.
type fileStat struct {
	name    string
	size    int64
	mode    FileMode
	modTime time.Time
	sys     syscall.Stat_t
}
```

fileStat结构体的常用方法如下所示：

```go
func (fs *fileStat) Size() int64        { return fs.size }
func (fs *fileStat) Mode() FileMode     { return fs.mode }
func (fs *fileStat) ModTime() time.Time { return fs.modTime }
func (fs *fileStat) Sys() any           { return &fs.sys }
```

> 例子

```go
import (
	"fmt"
	"os"
)

func printMessage(filePath string) {
	fileInfo, err := os.Stat(filePath)
	if err == nil {
		fmt.Printf("%T\n", fileInfo)
		fmt.Println("fileName:", fileInfo.Name())
		fmt.Println("fileSize:", fileInfo.Size())
		fmt.Println("mode:", fileInfo.Mode()) // 文件权限
		fmt.Println("isDirectory:", fileInfo.IsDir())
		fmt.Println("Last modify:", fileInfo.ModTime())
	} else {
		fmt.Println("err:", err.Error())
	}
}

func main() {	
	// absolute path
	path := "/Users/lye/leetcode/leetcode_go/go.mod"
	printMessage(path)
	//*os.fileStat
	//fileName: go.mod
	//fileSize: 28
	//mode: -rw-r--r--
	//isDirectory: false
	//Last modify: 2022-10-05 12:04:51.902065812 +0800 CST

	// relative path
	path = "go.mod"
	printMessage(path)
	//*os.fileStat
	//fileName: go.mod
	//fileSize: 28
	//mode: -rw-r--r--
	//isDirectory: false
	//Last modify: 2022-10-05 12:04:51.902065812 +0800 CST
}
```

> 文件权限

文件的权限一共用10个字符描述，有三种基本权限：

- r (read, 读取权限) 八进制表示：4
- w (write, 写权限) 八进制表示：2
- x (execute, 执行权限) 八进制表示：1
- \- 无任何操作 八进制表示：0

文件权限说明：

<img src="../.image/image-20221007192612949.png" alt="image-20221007192612949" style="zoom:80%;" /> 

如：-rwxrwxrwx:权限的八进制表示： 0777

> 文件路径

与文件路径相关的方法表示:

- filepath.IsAbs() 判断是否为绝对路径
- filepath.Rel() 获取相对路径
- filepath.Abs() 获取绝对路径
- path.Join() 拼接路径

# 文件常规操作

## 1. 创建目录

创建目录时，如果目录存在，则创建失败。Go语言提供了两种方法：

> os.Mkdir(): 仅创建单级目录

Mkdir create a new directory with the specified name and permission bits. If there is an error, it will be of type *PathError.

> os.MKdirAll(): 创建多级目录

```go
func main() {
	dirName := "./test1"
	// 创建目录
	err := os.Mkdir(dirName, os.ModePerm)
	if err == nil {
		fmt.Println("the directory was created successfully, dirName=", dirName)
		// the directory was created successfully, dirName= ./test1
	} else {
		fmt.Println("error:", err.Error())
	}
	path2 := "./test2/abc/xyz"
	// 创建多级目录
	err = os.MkdirAll(path2, os.ModePerm)
	if err == nil {
		fmt.Println("the directory was created successfully, dirName=", path2)
		//the directory was created successfully, dirName= ./test2/abc/xyz
	} else {
		fmt.Println("error:", err.Error())
	}
}
```

## 2. 创建文件

> os.Create()： 如果文件存在，则将其覆盖

`os.Create()` creates the named file with mode 0666, truncating it if it already exists. os.Create() --> *File

该函数本质上是调用`os.OpenFile()`函数，使用方式：

```go
func main() {
	dirName := "./test1"
	err := os.Mkdir(dirName, os.ModePerm)
	if err == nil {
		fileName := "./test1/abc.txt"
		file1, err := os.Create(fileName)
		if err == nil {
			fmt.Printf("create success %v", file1) 
			// create success &{0x14000122120}
		} else {
			fmt.Println("error:", err.Error())
		}
	} else {
		fmt.Println("error:", err.Error())
	}
}
```

## 3. 打开关闭文件

让当前的程序和指定的文件建立一个链接。

> os.Open()

Open() opens the named file for reading. If successful, methods on the returned file can be used for reading; the associated file descriptor has mode `O_RDONLY`. os.Open(fileName) --> *File

该函数本质上是调用`os.OpenFile()`函数. `os.OpenFile(filename, mode, perm) --> *File`

- filename : 文件名称
- mode: 文件打开方式，可以同时使用多个方式，用`|`隔开。

- perm: 文件权限。文件不存在时创建文件，需要指定权限。

<center><strong>文件打开方式</strong></center>

| 关键字   | 模式             |
| -------- | ---------------- |
| O_RDONLY | 只读模式         |
| O_WRONLY | 只写模式         |
| O_RDWR   | 读写模式         |
| O_APPEND | 追加模式         |
| O_RDONLY | 文件不存在就创建 |

> file.Close()

关闭文件，程序和文件之间的链接断开，通常与打开文件配对使用。

```go
func main() {
	fileName1 := "./test1/abc.txt"
	file1, err := os.Open(fileName1)
	if err == nil {
		fmt.Printf("open success, file: %v\n", file1)
		// open success, file: &{0x14000070120}
	} else {
		fmt.Println("error:", err.Error())
	}
	// 以读写的方式打开，如果文件不存在就创建
	fileName2 := "./test1/abc1.txt"
	file2, err := os.OpenFile(fileName2, os.O_RDWR|os.O_CREATE, os.ModePerm)
	if err == nil {
		fmt.Printf("open success, file: %v\n", file2)
		// open success, file: &{0x14000070180}
	} else {
		fmt.Println("error:", err.Error())
	}
	file1.Close()
	file2.Close()
}
```

## 4. 删除文件

删除文件有两种方法：

- os.Remove() 删除已命名的文件或**空目录**，如果删除的不是空目录，那么系统会报错
- os.RemoveAll() 移除所有的路径和他包含的任何子节点

```go
func main() {
	file1 := "./test1/abc.txt"
	err := os.Remove(file1)
	if err == nil {
		fmt.Printf("delete successful %s\n", file1)
		// delete successful ./test1/abc.txt
	} else {
		fmt.Println("error:", err.Error())
	}
	file2 := "./test1/abc1.txt"
	err = os.RemoveAll(file2)
	if err == nil {
		fmt.Printf("delete successful %s\n", file2)
		//delete successful ./test1/abc1.txt
	} else {
		fmt.Println("error:", err.Error())
	}
}
```

## 5. 读取文件

读取文件的步骤如下：

1. 打开文件

```go
os.Open(fileName)
```

2. 读取文件

从文件中开始读取数据，返回值 n 是十几读取的字节数，如果读取到文件末尾，n = 0 或 err为`io.EOF`.

3. 关闭文件

```go
func main() {
	fileName := "./test1/abc.txt"
	file, err := os.Open(fileName)
	if err == nil {
		bs := make([]byte, 1024, 1024) // buffers
		n := -1
		for {
			n, err = file.Read(bs)
			if n == 0 || err == io.EOF {
				fmt.Println("file read over")
				break
			}
			fmt.Println(string(bs[:n]))
		}
	} else {
		fmt.Println("error: ", err.Error())
	}
}
```

## 6. 写入文件

写入文件的步骤如下：

1. 打开或创建文件

```go
os.OpenFile()
```

2. 写入文件

```go
file.Write([]byte) n, err
file.WriteString([]byte) n, err
```

 n 表示写入的字节数

3. 关闭文件

```go
func main() {
	file, err := os.OpenFile("./test1/abc.txt", os.O_RDWR|os.O_APPEND|os.O_CREATE, os.ModePerm)
  // 追加模式
	defer file.Close()
	if err == nil { // 如果打开文件成功
		n, err := file.Write([]byte("abcdef12345\n"))
		if err == nil { // 如果写入文件成功
			fmt.Printf("write successful, n = %d\n", n)
		} else {
			fmt.Println("error:", err.Error())
		}
		// =====
		n, err = file.WriteString("测试\n")
		if err == nil {
			fmt.Printf("write successful, n = %d\n", n)
		} else {
			fmt.Println("error:", err.Error())
		}
	} else {
		fmt.Println("error:", err.Error())
	}
}
```

## 7.复制文件

> io.Copy() 复制文件

```go
func copyFile(srcFile, destFile string) (int64, error) {
	file1, err := os.Open(srcFile)
	if err != nil {
		return 0, err
	}
	file2, err := os.OpenFile(destFile, os.O_RDWR|os.O_CREATE, os.ModePerm)
	if err != nil {
		return 0, err
	}
	defer file1.Close()
	defer file2.Close()
	return io.Copy(file2, file1)
}
```

```go
func main() {
	total, err := copyFile("./test1/abc.txt", "./test1/abc_1.txt")
	if err != nil {
		fmt.Println("error:", err.Error())
	} else {
		fmt.Printf("copy finish, %d\n", total)
	}
}
```

# ioutil

ioutil包的核心函数如下所示

| 方法              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| ReadFile() []byte | 读取文件中的所有数据,返回读取的字节数组                      |
| WriteFile()       | 向指定文件写入数据，如果文件不存在，则创建文件，写入数据前晴空文件 |
| ReadDir()         | 读取一个目录下的子内容(子文件和子目录名称)，但是仅读取一层   |
| TempDir(fileName) | 在当前目录下，创建一个名为`fileName`前缀的临时文件夹，并返回文件夹路径 |
| TempFile()        | 在当前目录下，创建一个名为`fileName`前缀的临时文件，并以读写模式打开文件，并返回`os.File`指针对象 |

```go
```

# bufio

