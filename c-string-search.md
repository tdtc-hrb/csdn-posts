---
title: "查找字符串"
description: "使用C语言实现"
date: 2020-05-04T16:08:08+08:00
---
我们经常要用到查找某个字符串的功能，使用C++或者其他高级语言实现起来很简单；但是使用C语言实现就不那么easy了。

# 切割字符串
strtok函数
```c
char *strtok_s(
   char *strToken,
   const char *strDelimit,
      char **context
);
```
- strToken    
  要切割的字符串
- strDelimit     
  分隔符
- context    
  为空的字符串地址

# 匹配字符串
strstr函数
```c
char *strstr(
   const char *str,
   const char *strSearch
); // C only
```
- str     
  被搜索的字符串
- strSearch     
  要搜索的字符串


# 函数getStrVal_s
```c
char *getStrVal_s(char *str, char key[])
{
	char* strDelimit = ";";
	char *strSeq = "";
	char *context = NULL;

	char *token = strtok_s((char*)str, strDelimit, &context);
	while (token != NULL) {
		char* temp = strstr(token, key);
		if (temp != NULL)
			strSeq = temp;

		if (token != NULL) {
			token = strtok_s(NULL, strDelimit, &context);
		}

	}

	return strSeq;
}
```

## 主程序
```c
int _tmain(int argc, _TCHAR* argv[])
{

    char string[] = "True democracy demands that citizens cannot be thrown in jail because of what they believe;and that businesses can be opened without paying a bribe;It depends on the freedom of citizens to speak their minds and assemble without fear;and on the rule of law and due process that guarantees the rights of all people";
	char* val = getStrVal_s(string, "freedom");

	printf("val: %s", val);

	return 0;
}
```
