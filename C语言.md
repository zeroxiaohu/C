# 1 C语言基础知识
## 1.1 数据类型 
    char short int long signed unsigned float double sizeof
## 1.2 运算和控制
    + - * / 

    while

    do while

    if else 

    switch case continue break

## 1.3 数据存储
    static extern const volatile
## 1.4 结构
    struct enum union typedef
## 1.5 位操作和逻辑运算
    <<   >>     &    |    ~   ^
## 1.6 预处理
    #define  #include   #if...#elif...#else...#endif
## 1.7 位 字节 字的关系
	位(bit):计算机中最基本的单位,对应0或1
	字节(Byte):计算机中数据存储的基本单位,8位=1字节
	字(Word):计算机进行数据处理和运算的单位。32位机中,4字节=1字;64位机中,8字节=1字;
# 2 进制转换
``` c
    int a = 021;  //oct 8进制表示前方加 0不是o
    int b = 17;   //dec 十进制表示
    int c = 0x11; //hex 16进制表示前方加 0x
    int d = 0b10001;// 二进制表示前方加 0b
```
# 3 位运算
null
# 4 封装寄存器
```c
/*
	lesson4：封装寄存器
*/

typedef struct 
{
	volatile unsigned int  CRL;
	volatile unsigned int  CRH;
	volatile unsigned int  IDR;
	volatile unsigned int  ODR;
	volatile unsigned int  BSRR;
	volatile unsigned int  BRR;
	volatile unsigned int  LCKR;
}GPIO_TypeDef;

#define GPIOA  ((GPIO_TypeDef *)  0x40010800)
#define GPIOB  ((GPIO_TypeDef *)  0x40010C00)
#define GPIOC  ((GPIO_TypeDef *)  0x40011000)

int main(void)
{
	GPIOB->ODR |= (1<<7) | (1<<9);
	return 0;
}
```
# 5 函数指针
```c
/*
	lesson5：函数指针
*/

//加法函数
int add(int a,int b)
{
	return a+b;
}

//减法函数
int sub(int a,int b)
{
	return a-b;
}

# if 1

//定义函数指针别名
typedef int (*pfun)(int, int); 

//计算函数
int calc(pfun fp, int a, int b)  
{
  return fp(a,b);
}

#else

	//不定义函数指针别名
	//typedef int (*pfun)(int, int); 

	//计算函数
	int calc(int (*pfun)(int, int), int a, int b)  
	{
	  return pfun(a,b);
	}

#endif

int main(void)
{
	int c;
	
	c=calc(add, 5, 3); //加法

	c=calc(sub, 5, 3); //减法
	
	return 0;
}

```
# 6 链表
```c
/*
	lesson6：链表
*/

/*------------------------1.链表和结点的定义----------------------------*/

/*结点结构体*/
typedef struct LIST_NODE {
	int data;                        /*用于存放结点数据*/
	struct LIST_NODE *pxNext;        /*用于指向下一个结点*/
	struct LIST_NODE *pxPrevious;    /*用于指向上一个结点*/
}ListNode;


/*链表结构体*/
typedef struct LIST {
	unsigned int NumberOfNodes;      /*用于记录链表结点数量*/
	ListNode RootNode;               /*用于作为循环链表的参考点*/
}List;

/*------------------------2.链表和结点的初始化---------------------------*/
/*结点初始化*/
void ListInitialiseItem(ListNode *pxListNode, int value)
{  
	pxListNode->data = value;       /*结点数据赋值*/
}

/*链表初始化*/
void ListInitialise(List *pxList)
{
	pxList->RootNode.pxNext = &(pxList->RootNode);     /*由于此时链表中没有结点,第一个结点指向自己*/
	pxList->RootNode.pxPrevious = &(pxList->RootNode); /*由于此时链表中没有结点,第一个结点指向自己*/
	
	pxList->NumberOfNodes = 1;                         /*链表结点计数初始化为1,也就是只有一个根结点*/     
}

/*------------------------3.1结点插入链表---------------------------*/
void ListInsertEnd(List *pxList, ListNode *pxInsertListNode)
{
	ListNode *pxNextNode = &(pxList->RootNode);                /*插入结点的后结点*/
	ListNode *pxPreviosNode = pxList->RootNode.pxPrevious;     /*插入结点的前结点*/

	
    pxInsertListNode->pxNext = pxNextNode;                     /*插入结点指向后结点*/
	pxInsertListNode->pxPrevious = pxPreviosNode;              /*插入结点指向前结点*/ 
	
	pxPreviosNode->pxNext = pxInsertListNode;                  /*前结点指向插入结点*/   
    pxNextNode->pxPrevious = pxInsertListNode;                 /*后结点指向插入结点*/   

   (pxList->NumberOfNodes)++;                                  /*链表结点计数加1*/
}

/*------------------------3.2链表删除结点---------------------------*/
void ListRemove(List *pxList, ListNode *pxIListToRemove)
{
	ListNode *pxPreviosNode = pxIListToRemove->pxPrevious;     /*删除结点的前结点*/
	ListNode *pxNextNode = pxIListToRemove->pxNext;            /*删除结点的后结点*/
	
    pxNextNode->pxPrevious = pxPreviosNode;                    /*后结点指向前结点*/
    pxPreviosNode->pxNext = pxNextNode;                        /*前结点指向后结点*/

	(pxList->NumberOfNodes)--;                                 /*链表结点计数减1*/
}


int main(void)
{
	/*1.定义链表、结点*/
	List      list;         //定义链表
	ListNode  list_node1;   //定义结点1
	ListNode  list_node2;   //定义结点2

	/*2.初始化链表、结点*/
	ListInitialise(&list);
	
	ListInitialiseItem(&list_node1, 100);
	ListInitialiseItem(&list_node2, 200);
	
	/*3.插入链表*/
	ListInsertEnd(&list, &list_node1);
	ListInsertEnd(&list, &list_node2);
	
	/*4.删除结点*/
	ListRemove(&list, &list_node1);
	
	return 0;
}

```
# 7 扩展