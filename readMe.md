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
    
    太简单，略    

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
touch $file
# 是普通
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