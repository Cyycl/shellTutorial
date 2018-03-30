# 1. 变量

- 变量的赋值时，`=`之间不能有空格
- 使用变量时建议加上`{}`
- 单引号和双引号的区别：
    - 单引号对于包裹的内容，会原样输出，即使是变量或命令
    - 双引号对于包裹的内容，会先进行解析，再输出
    
    ```sh
    echo 'ls is `ls -al`' # ls is `ls -al`
    echo "ls is `ls -al`"
    ```
- 反引号和`$()`
    - 支持将命令的执行结果赋值给变量, 推荐使用`$()`
- 使用`readonly`可以将变量设为只读, 例如 `readonly myUrl`
- 删除变量。使用`unset`可以删除变量 ， **变量被删除后不能再次使用**
- 变量类型
    - 环境变量
    
        所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
    - 局部变量
    
        局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量        

    - shell变量
        
        shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行
        
- 特殊的变量
    
    ```sh
    #!/bin/bash
    echo "filename $0" # 当前脚本文件名
    echo "first arg $1" # $n 表示第n个输入参数
    echo "secode arg $2"
    echo "all args \$\*  $*" # 表示所有输入参数
    echo "all args \$\@ $@" # 表述所有输入参数
    echo "count of args $#" # 输入参数个数
    ```
    
    - $* 和 $@ 区别, 推荐使用`$@`
        - $* 和 $@ 输出均为："$1" "$2" ... "$n"
        - "$*"输出为："$1 $2 ... $n"
        - "$@"输出为："$1" "$2" ... "$n"
    

# 2. 替换
    
- 命令替换
        
    使用反引号`command` 或 `$(command)`
        
- 变量替换
        
    暂时掌握 `$a` 和 `${a}`

# 3. 运算符

- 算术运算符

    
    ```
    `expr expression` # expression中运算符之间必须有空格 
    ```
    
    等价于 `$((expression))` , 但是这里**运算符之间不能有空格**
    
    ```
    a=4
	b=6
	add=`expr ${a} + ${b}` # 等价 add=$(($a+$b))
	subtract=`expr ${b} - ${a}` 
	multiply=`expr ${a} \* ${b}` # 等价于 multiply=$(($a*$b))
	division=`expr ${b} / ${a}`
	echo $add
	echo $subtract
	echo $multiply
	echo $division
    ```

- 关系运算符
    
    关系运算符只适用于数字类型，或者字符串的值是数字
    - -eq: `[$a -eq $b]` a 和 b 相等返回true
    - -ne: `[$a -ne $b]` a 和 b 不相等返回true
    - -gt: `[$a -gt $b]` a 大于 b 返回true
    - -lt: `[$a -lt $b]`
    - -ge: `[$a -ge $b]` a 大于等于 b 返回true
    - -le

- 布尔运算符
    
    - !: 非运算符 
    - -o：或运算符
    - -n：与运算符

    ```
    a=9
    b=4
    # 以下均为true
    # ! 非
    if [ $a != $b ] 
    then
      echo 'a is not equal to b'
    fi
    # -o 或
    if [ $a -gt 5 -o $b -gt 5 ] 
    then
      echo 'a or b is greater than 5'
    fi
    # -a 且
    if [ $a -le 10 -a $b -lt 10 ]
    then
      echo 'a and b are both less than 10'
    fi 
    ```

- 字符串运算符
    
    - ==: 字符串相等，返回true
    - !=: 字符串不等，返回true
    - -z：字符串长度为0， 返回true
    - -n: 字符串长度不为0，返回true

    ```sh
    a="abc"
    b=""
    # -z
    if [ -z $b ] 
    then
      echo 'b is empty string'
    fi
    # -n
    if [ -n $a ]
    then
      echo 'a is not empty'
    fi
    ```

- 文件测试运算符
    
    文件测试运算符用于检测文件的各种属性
    
    - -d: 检测文件是否是目录，是目录，返回true
    - -f：检测文件是否是普通文件，是普通文件，返回true
    - -r：检测文件是否可读，有读权限，返回true
    - -w：检测文件是否可写， 有写权限，返回true
    - -x：检测文件是否可执行，有执行权限，返回true
    - -s：检测文件是否为空，，**不为空返回true**
    - -e：检测文件，包括目录，是否存在, **存在返回true**
    

	``` sh
	pwd=`pwd`
	file="$pwd/a.sh"
	# 文件不存在
	if [ ! -e $file ]
	then
	  echo 'file is not exist'
	fi
	touch $file # 创建文件
	# 是普通文件
	if [ -f $file ] 
	then
	  echo "file is a normal file"
	fi
	# 不是文件夹
	if [ ! -d $file ]
	then
	  echo "file is not a dictionary"
	fi
	# 可读
	if [ -r $file ]
	then
	  echo "file is readable"
	fi
	# 可写
	if [ -w $file ]
	then
	  echo "file is writable"
	fi
	chmod +x $file # 给u,g,o都赋予x权限
	# 可执行
	if [ -x $file ]
	then
	  echo "file is executable"
	fi
	echo 'hahaha' > $file # 向a.sh写入内容
	# 文件不为空
	if [ -s $file ]
	then
	  echo "file's size is not zero "
	fi
	```
	
# 4. 常用命令

- echo

	`echo -n `: 打印不换行
	
	栗子：
	
	```
	echo -n "hello"
	echo "world" # hello world
	```
	
- printf
	
	格式：`printf format-string [argument,...]`
	
	```
	a="abc"
	b=15
	printf "%s and %d\n" $a $b
	```

- test
	
	`test` 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。实际使用不多, 个人更倾向于使用`[ expression ]` 代替 `test`	
	
	```
	a=5
	b=10
	if test $a -lt $b 
	then
  		echo "a is bigger than b"
	fi
	```

# 5. 逻辑语句

- If
	
	- if-then-if
		
		格式：
		
		```
		if [ expression ]
		then
  			Statement(s) to be executed if expression is true
		fi
		```
	- if-then-else-if
		
		格式：
	
		```
		if [ expression ]
		then
   			Statement(s) to be executed if expression is true
		else
   			Statement(s) to be executed if expression is not true
		fi
		```
	- if-then-elif-then-if
	
		格式：
		
		```
		if [ expression 1 ]
		then
   			Statement(s) to be executed if expression 1 is true
		elif [ expression 2 ]
		then
   			Statement(s) to be executed if expression 2 is true
		elif [ expression 3 ]
		then
   			Statement(s) to be executed if expression 3 is true
		else
   			Statement(s) to be executed if no expression is true
		fi
		```

- Case
	
	- case-in-esac
		
		格式：
		
		```
		case 值 in
		模式1)
		    command1
		    command2
		    command3
		    ;;
		模式2）
		    command1
		    command2
		    command3
		    ;;
		*)
		    command1
		    command2
		    command3
		    ;;
		esac
		```
		栗子：
		
		```
		a="abcd"
		case ${#a} in
		  1)
		    echo "length is 1"
		  ;;
		  2)
		    echo "length is 2"
		  ;;
		  3)
		    echo "length is 3"
		  ;;
		  *) # default
		    echo "not match"
		  ;;
		esac
		```

- For
	
	- for-in-do-done
		
		格式：
		
		```
		for 变量 in 列表
		do
		    command1
		    command2
		    ...
		    commandN
		done
		```
		栗子：
	
		```
		for value in 1 2 3
		do
	  		echo "value is ${value}"
		done
		```

- While
	
	- while-do-done
	
		格式：
		
		```
		while command
		do
   			Statement(s) to be executed if command is true
		done
		```
		栗子：
		
		```
		a=0
		while [ $a -lt 5 ]
		do
  			echo "value is ${a}"
  			a=`expr ${a} + 1`
		done
		```

- break
	
	栗子：
	
	```
	while read n
	do
	  case $n in
	    1|2|3|4)
	      echo "num is in (1,2,3,4), game is continue"
	    ;;
	    *)
	      echo "num is not in (1,2,3,4), game over"
	      break # 跳出循环
	    ;;
	  esac
	done
	```

# 6. 数据类型

- 字符串
	
	略

- 数组
	
	掌握`数组定义`,`数组读取`和`数组长度获取`
	
	```
	array=(1 2 3 4 "abcd") # 数组定义，记住：1. 使用空格分割不同值；2. 可以存放不同类型的数据
	echo ${array[0]} # 数组值获取，一定要使用{}包住数组变量
	echo ${array[1]}
	echo ${array[*]} # 返回全部值，* 和 @ 的意义详见第一节中的特殊变量
	echo ${array[@]} # 返回全部值
	
	array[1]=5 # 数组赋值
	echo ${array[1]}
	
	echo "数组元素的个数是 ${#array[*]}" # ${#var}是获取变量var的长度
	echo "数组第5个元素的长度是 ${#array[4]}"
	```
	
- **函数**
	
	格式：
	
	```
	function_name () {
    	list of commands
    	[ return value ]
	}
	function_name  arg1 arg2 ... # 函数调用，不需要加括号
	```
	
	- 函数参数
		
		- $#: 函数参数个数
		- $*: 函数全部参数
		- $@: 函数全部参数，但是在""下返回值与$*不同，详见特殊变量一节，$@更常用
		- $?: 函数返回值

		```
		add() {
		  echo "number of input params is $#" # 2
		  echo "all input params is $@" # 2 3
		  a=$1
		  b=$2
		  return $(($a+$b))
		}
		add 2 3
		result=$? # add函数返回值
		echo "result is $result" # 5
		```
		
# 7. 输出输入重定向

- 输出重定向
	
	- `>` 
		
		```
		command > txt # 每次覆盖写入txt
		```
		
	- `>>`
	
		```
		command >> txt # 每次追加到txt的尾部
		```
- 输入重定向

	- `<`
		
		栗子：
		
		```
		wc -l test.sh 等价于 wc -l < test.sh
		```

- 重定向深入讲解
	
	Linux命令运行时，默认会打开三个文件：
	
	- 标准输入文件`stdin`, 文件描述符为**0**，程序会默认从`stdin`流中读取输入信息
	- 标准输出文件`stdout`， 文件描述符为**1**，程序会默认从`stdin`流中输入信息
	- 标准错误文件`stderr`，文件描述符为**2**，程序会将错误信息写入`stderr`流中
	
	栗子：
	
	其中 `m` 和 `n` 是文件描述符
	 
	- m > file(覆盖写入file) 或 m >> file（追加在file的末尾）
		
		```
		./test.sh > test.log # 这里m默认为1， 将`stdout`重定向`test.log`
		./test.sh 2 > test.log # 将`stderr`重定向到`test.log`
		```
	- file m>&n 

		将输出文件`m`和`n`合并， 再重定向到file
		
		```
		./test.sh > test.log 2>&1 # 将`stderr`和`stdout`合并后，重定向到`test.log`		
		```		
	- /dev/null (起到”禁止输出“的效果)
	
		执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到`/dev/null`
		
		`/dev/null`是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。
		
		```
		./test.sh > /dev/null 2>&1 # 屏蔽 stdout 和 stderr
		```

# 8. shell文件包含

Shell 也可以包含外部脚本，将外部脚本的内容合并到当前脚本。
	
实现方式：
	
- `. filename`: 注意前面有一个`.`，`.` 和 filename之间有空格
- `source filename`

栗子：

```
# fileA
#!/bin/bash
name="jason"

# fileB
#!/bin/bash
. ./fileA
echo "$name chen" # jason chen

```


	


	
		