# OperatorAssigenment
为一个类添加赋值运算符函数

#pragma once

#include <iostream>
using namespace std;

//如下为类型CMyString的声明，为该类型添加赋值运算符函数
class CMyString
{
public:
	CMyString(char *pData = NULL);
	CMyString(const CMyString & str);
	~CMyString(void);

	//赋值运算符重载的两种写法
	CMyString & operator=(CMyString str);
	//CMyString & operator=(const CMyString & str);
private:
	char *m_pData;
};

//构造函数
CMyString::CMyString(char *pData)
{
	if (pData != NULL)
	{
		//多开辟一个空间，用来存放'\0'
		m_pData = new char[strlen(pData) + 1];
		strcpy(m_pData, pData);
	}
	else
	{
		m_pData = new char[1];
		m_pData[0] = '\0';
	}
}

//拷贝构造
CMyString::CMyString(const CMyString & str)
{
	m_pData = new char[strlen(str.m_pData) + 1];
	strcpy(m_pData, str.m_pData);
}

//析构函数
CMyString::~CMyString(void)
{
	//如果m_pData不为空，就释放空间
	if (m_pData)
	{
		delete[] m_pData;
		m_pData = NULL;
	}
}

//赋值运算符重载
//传统写法
//1.返回值应该声明为该类型的引用，只有返回引用才可以连续赋值
//2.参数应声明为常量引用，如果参数不是引用而是实例，从形参到实参就会调用一次拷贝构造，使用引用可以减少消耗，提高效率
//3.注意释放自身已有内存，避免内存泄漏
//4.注意是否自己为自己赋值
//
//CMyString & CMyString::operator=(const CMyString & str)
//{
//	/*if (m_pData != str.m_pData)*/
//	if (this != &str)
//	{
//		delete[]m_pData;
//		m_pData = new char[strlen(str.m_pData) + 1];
//		strcpy(m_pData, str.m_pData);
//	}
//
//	return *this;
//}

//现代写法
//拷贝构造一个匿名对象，交换对象的m_pData指针得到想要拷贝的内容
//匿名对象出作用域会调用析构函数，从而达到释放原有内存的空间，不会造成内存泄露
CMyString & CMyString::operator=(CMyString str)
{
	std::swap(m_pData, str.m_pData);

	return *this;
}

//测试用例
void testOperatorAssignment()
{
	//CMyString str("abcdef");
	//CMyString str1(str);
	//CMyString str2("123");

	//str2 = str1;

	CMyString str("abcdef");
	CMyString str1("abc");
	CMyString str2("123");

	str2 = str1 = str;
}
