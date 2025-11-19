
# stdio.h：文件操作
## 特殊文件
三个默认打开的文件流
* `stdin`：标准输入，从键盘读取数据
* `stdout`：标准输出，写数据到屏幕
* `stderr`：标准错误，也是屏幕但专门用来输出错误信息

---

## 文件管理：打开、关闭和删改文件
1. `fopen`：打开文件
	* 功能：打开文件，返回一个FILE* 指针，后续对该文件操作依靠该指针
	* `FILE* fp = fopen(const char* filename,const char* mode)
	* 打开模式：“a”追加、“w“写入（清空所有内容）、”r“只读、带+则可读可写
	* 成功返回指向FILE的指针，失败返回NULL，同时设置errno
2. `fclose`：关闭文件
	* 用完文件后一定要关，不然会丢失数据
	* `FILE* fclose(FILE* stream)`
	* 成功返回0；失败EOF，并设置errno
3. `freopen`：重定向文件流
	* 功能：关闭`stream`原有文件，打开`filename`并重新关联到`stream`
	* `FILE* freopen(const char* filename, const char* mode,FILE* stream)`
	* 成功返原指针；失败返回NULL，设置errno
	* 先调用fclose关闭原有文件，再以mode打开filename，将新文件的FILE信息覆盖到stream指向的结构体
4. `remove`：删除文件
	* 功能：直接删掉文件
	* `int remove(const char* filename)`
5. `rename`：重命名/移动文件
	* 给文件改名，或者挪到别的文件夹
	* `int rename(const char* old, const char* new)`

---

## 格式化输入输出
输出：
1. `printf`
2. `fprintf`：打印到文件
	* 功能和printf类似，打印到文件里
	* `int fprintf(FILE* stream, const char*format...)`
	* 输出目标是任意stream，不局限于文件
3. `sprintf`：字符串格式化
	* 功能：把数据按格式存到字符串中
	* `int sprintf(char* buffer,const char* format...)`

输入：
1. scanf
2. `fscanf`：从文件读取数据
	* 功能：和scanf类似；从文件读取
	* `int fscanf(FILE* stream,const char *format...)`
3. `sscanf`：从字符串读数据
	* 功能：从一个字符串里解析出数据
	* `int sscanf(const char* buffer,const char* format...)`
4. `vscanf`：自定义输入（这个具体见<stdarg.h>头文件的解读）
	* `int vscanf(const char* restrict format,va_list vlist)`
	* 用于自定义可变参数函数时转发参数到格式化输入逻辑
---

## 字符输入输出函数
1. `fgetc`：从流读入单个字符
	* `int fetc(FILE* stream)`
	* 成功返回读取的字符，失败/文件结束返回EOF
2. `getc`和`getchar`
	* `getc`：`#define getchar(stream)`功能与`fgetc`完全一致
	* `getchar`：专门从键盘读取字符
	* 成功时，返回获取的字符（作为转换为int型的无符号字符）。失败时，返回EOF
3. `fputc`：写入一个字符
	* 往文件里写入一个字符
	* `int futc(int ch,FILE* stream)`
	* putchar：专门往屏幕里写字符
4. `fgets`： 读入一行文字
	* 从文件流读入一行文字，包括换行符，存到数组里。
	* `char* fgets(char* str,int count,FILE* stream)`
	* 成功时返回字符串，失败时返回空指针
5. `fputs`：写一行文字
	* 功能：文件流写入一串字符，**不会自动加换行**
	* `int fputs(const char* str,FILE* stream)`
	* 成功返回非负值，失败返回EOF
	* `puts`：专门往屏幕里写，写完**自动加一个换行符**


## 文件定位
打开文件后，读写位置在开头（除了追加模式在末尾）

1. `fseek`：跳转位置
	* 功能：告诉电脑“接下来从文件的XX位置开始读写”（将文件流stream的文件位置指示器设置为offset所指向的值）
	* `int fseek(FILE* stream,long offset,int origin)`
		* offset：偏移量，相对于远点偏移的数量
		* stream：流，要修改的文件流
		* origin：起点，添加偏移量的位置，有以下值：SEEK_SET、SEEK_CUR、DEEK_END
	* 成功时返回0，否则返回非零值
2. `ftell`：查看当前位置
	* 功能：告诉你现在的读写位置在相对开头的第几个字节
	* `long ftell(FILE* stream)`
	* 成功时返回文件位置指示器，失败时返回-1L
3. `rewind`：回到开头
	* 功能：直接把读写位置跳回文件最开头
	* `void rewind(FILE* stream)`
---

## 缓冲控制函数

`fflush`：刷新缓冲区
* `int fflush(FILE* stream)`
* stream应是输出流
* 将缓冲区已有的数据强制输出，并清空缓冲区