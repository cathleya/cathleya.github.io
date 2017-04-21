---
layout: post
title: "Quickly learn shell script"
modified:
categories: blog
excerpt:
tags: [Shell,Unix]
image:
  feature:
date: 2014-10-23T15:39:55-04:00
modified: 2016-06-01T14:19:19-04:00
---



# Quickly learn shell script

### 1.main.sh

```

# touch hello.sh -> chmod +x main.sh -> ./main.sh

#!/bin/sh

cur=$(pwd)

echo $cur

echo 'hello world'

source ./good.dd/good.sh
. ./good.dd/good.sh

#string
string="abc-adf.xcworkspace"
echo ${#string}
echo ${string:0:5}

#get file name without Suffix
path=`find *.sh`
echo $path
name=${path%.*}
echo $name

# get directory name without suffix
path=`find *.xcworkspace -maxdepth 0`
echo $path
pname=${path%.*}
echo $pname

# if else 
a=30
b=20
if [[ $a>$b ]]; then
	echo happy
else
	echo sad
fi

# case
abc=3
case ${abc} in
	1 ) echo me
		exit 0
		;;
	2 ) echo you
		exit 1
		;;
	3 ) echo she
		exit 2
		;;
	* ) echo "Bad option, please choose again"
esac


# for
for (( i = 0; i < 10; i++ )); do
	echo $i
done

for i in happy words at your mouth; do
	echo "im good at ${i} skills"
done

for i in $(ls //Users); do
	echo $i
done

# until
until [[ $a<$b ]]; do
	echo laughing
	a=1
done

# while
while true
do
	echo crying
done

while [[ $a>$b ]]; do
	echo sad
done

while :
do
	echo very happy
done



```



### 2.good.dd/good.sh

```
#!/bin/sh

echo "i'm good"

function good(){
	echo "i'm really good"
}

good

#$1 is value was inputed by users，eg: good.sh
real_path=`readlink -f $1`

echo $real_path

awk

sed

xargs

curl



```

### 3.learn more

[Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/)
 [Unix Shell Programming](Unix Shell Programming)



#### 4. useful code 

```
find . "(" -name "*.m" -or -name "*.h"  ")" -print | xargs wc -l
```


-------

[TOC]

