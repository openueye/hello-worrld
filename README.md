#define _CRT_SECURE_NO_WARNINGS 1
#include <stdio.h>
#include <stdlib.h>
/* 宏定义 */
#define STACK_INIT_SIZE  100       
#define STACKINCREMENT   10  

/* 状态码 */
#define TRUE        1
#define FALSE       0
#define OK          1
#define ERROR       0
#define INFEASIBLE  -1
#define OVERFLOW    -2

typedef int Status;
typedef int SElemType;

typedef struct
{
    SElemType* base;
    SElemType* top;
    int stacksize;
}SqStack;

/* 初始化顺序栈 */
Status InitStack(SqStack* S)
{   //构造一个空栈  
    S->base = (SElemType*)malloc(STACK_INIT_SIZE * sizeof(SElemType));
    if (!S->base) exit(OVERFLOW);
    S->top = S->base;
    S->stacksize = STACK_INIT_SIZE;
    return OK;
}

/* 销毁顺序栈 */
Status DestoryStack(SqStack* S)
{   // 销毁栈S, S不再存在
    if ((*S).base) free((*S).base);
    (*S).base = NULL;
    (*S).stacksize = 0;
    return OK;
}

/* 清空栈S */
Status ClearStack(SqStack* S)
{
    // 把S置为空栈
    (*S).base = (*S).top;
    return OK;
}

/* 判断栈S是否为空 */
Status StackEmpty(SqStack S)
{
    if (S.base == S.top) return TRUE;
    else return FALSE;
}

/* 返回栈的长度 */
Status StackLength(SqStack S)
{
    if (S.base) return S.top - S.base;
}

/* 取栈顶元素 */
Status GetTop(SqStack S, SElemType* e)
{    // 若栈不空, 则用e返回S的栈顶元素, 并返回OK;
     //否则返回ERROR
    if (S.top == S.base) return ERROR;
    *e = *(S.top - 1);
    return OK;
}

/* 压栈 */
Status Push(SqStack* S, SElemType e)
{
    // 插入元素e为新的栈顶元素
    if ((*S).top - (*S).base >= (*S).stacksize)
    {    // 栈满, 追加存储空间
        (*S).base = (SElemType*)realloc((*S).base, (static_cast<unsigned long long>((*S).stacksize) + STACKINCREMENT) * sizeof(SElemType));
        if (!(*S).base) exit(OVERFLOW);
        (*S).top = (*S).base + (*S).stacksize;
        (*S).stacksize += STACKINCREMENT;
    }
    *(*S).top++ = e;
    return OK;
}

/* 弹栈 */
Status Pop(SqStack* S, SElemType* e)
{   // 若栈不空, 则删除S的栈顶元素, 用e返回其值, 并返回OK; 否则返回ERROR
    if ((*S).top == (*S).base) return ERROR;
    *e = *(--(*S).top);
    return OK;
}

/* 打印栈S */
Status ShowStack(SqStack S)
{   // 遍历栈S并打印
    for (int i = 0; i < StackLength(S); ++i) {
        printf("%d ", S.base[i]);
    }
    return OK;
}

/*顺序栈的实现及栈的基本操作*/
int main()
{   // 创建顺序栈S
    SqStack S;
    // 判断初始化
    if (InitStack(&S) == OK)
        printf("顺序栈初始化完成.\n");
    else {
        printf("初始化失败，请重试.\n");
        return 0;
    }
    int n;
    int e = 0;
    printf("请输入顺序栈的长度.\n");
    scanf("%d", &n);
    printf("你将得到一个长度为%d的顺序栈.\n", n);
    printf("请输入%d个数据元素, 以回车为间隔.\n", n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &e);
        Push(&S, e);
    }

    /*打印顺序栈*/
    printf("输入完成, 你的顺序栈为:\n");
    ShowStack(S);

    /*打印栈顶元素*/
    int t = 0;
    GetTop(S, &t);
    printf("栈顶元素值为: %d.\n", t);

    /*弹栈*/ 
    int p = 0;
    Pop(&S, &p);
    printf("弹栈操作完成, 弹栈后的顺序栈如下:\n");
    ShowStack(S);

    /*清空顺序栈*/
    printf("操作完成, 清空顺序栈.\n");
    if (ClearStack(&S) == OK)
        printf("已清空顺序栈.\n");
    else {
        printf("清空顺序栈失败, 请检查.\n");
        return 0;
    }

    /*销毁顺序栈*/
    printf("操作完成, 销毁顺序栈.\n");
    if (DestoryStack(&S) == OK)
        printf("已销毁顺序栈.\n");
    else {
        printf("销毁顺序栈失败, 请检查.\n");
        return 0;
    }

    system("pause");
    return 0;
}

