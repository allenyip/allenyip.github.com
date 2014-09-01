---

title: 线性表

layout: post

---

##**术语**

* **数据** 数据是对客观事物的符号表示。

* **数据元素** 数据元素是数据的基本单位，可由若干不可分割的**数据项**组成。（数据元素与数据项可比喻成数据表中的记录和字段

* **数据对象** 数据对象是性质相同的数据元素的集合，是数据的一个子集。

* **数据结构** 数据结构是相互之间存在一种或多种特定关系的数据元素的集合。

根据数据元素之间的关系通常有以下四种数据结构：集合、线性结构、树形结构和图（网）状结构。


##**线性表**

线性表是多个数据元素的有限序列，具有以下几个性质：

* **同一性** 用于存放某一特定类型的元素。

* **序偶性** 除第一个元素外，其他每一个元素有且仅有一个直接前驱；除最后一个元素外，其他每一个元素有且仅有一个直接后继。

* **索引性** 元素在线性表中的“下标”唯一地确定该元素在表中的相对位置。

根据存储的特点可分为顺序表和链表，顺序表用一组地址连续的存储单元依次存储数据元素。

```c
/* 线性表的动态分配顺序存储结构 */
#define LIST_INIT_SIZE 10 /* 线性表存储空间的初始分配量 */
#define LIST_INCREMENT 2 /* 线性表存储空间的分配增量 */
typedef struct
{
  ElemType *elem; /* 存储空间基址 */
  int length; /* 当前长度 */
  int listsize; /* 当前分配的存储容量(以sizeof(ElemType)为单位) */
}SqList;
```

基本操作包括：

```c
/* 顺序表示的线性表的基本操作(12个) */
void InitList(SqList *L) /* 算法2.3 */
{ /* 操作结果：构造一个空的顺序线性表L */
  L->elem=malloc(LIST_INIT_SIZE*sizeof(ElemType));
  if(!L->elem)
    exit(OVERFLOW); /* 存储分配失败 */
  L->length=0; /* 空表长度为0 */
  L->listsize=LIST_INIT_SIZE; /* 初始存储容量 */
}

void DestroyList(SqList *L)
{ /* 初始条件：顺序线性表L已存在。操作结果：销毁顺序线性表L */
  free(L->elem);
  L->elem=NULL;
  L->length=0;
  L->listsize=0;
}

void ClearList(SqList *L)
{ /* 初始条件：顺序线性表L已存在。操作结果：将L重置为空表 */
  L->length=0;
}

Status ListEmpty(SqList L)
{ /* 初始条件：顺序线性表L已存在。操作结果：若L为空表，则返回TRUE，否则返回FALSE */
  if(L.length==0)
    return TRUE;
  else
    return FALSE;
}

int ListLength(SqList L)
{ /* 初始条件：顺序线性表L已存在。操作结果：返回L中数据元素个数 */
  return L.length;
}

Status GetElem(SqList L,int i,ElemType *e)
{ /* 初始条件：顺序线性表L已存在,1≤i≤ListLength(L)。操作结果：用e返回L中第i个数据元素的值 */
  if(i<1||i>L.length)
    return ERROR;
  *e=*(L.elem+i-1);
  return OK;
}

int LocateElem(SqList L,ElemType e,Status(*compare)(ElemType,ElemType))
{ /* 初始条件：顺序线性表L已存在，compare()是数据元素判定函数(满足为1，否则为0) */
  /* 操作结果：返回L中第1个与e满足关系compare()的数据元素的位序。 */
  /*           若这样的数据元素不存在，则返回值为0。 */
  ElemType *p;
  int i=1; /* i的初值为第1个元素的位序 */
  p=L.elem; /* p的初值为第1个元素的存储位置 */
  while(i<=L.length&&!compare(*p++,e))
    ++i;
  if(i<=L.length)
    return i;
  else
    return 0;
}

Status PriorElem(SqList L,ElemType cur_e,ElemType *pre_e)
{ /* 初始条件：顺序线性表L已存在 */
  /* 操作结果：若cur_e是L的数据元素，且不是第一个，则用pre_e返回它的前驱， */
  /*           否则操作失败，pre_e无定义 */
  int i=2;
  ElemType *p=L.elem+1;
  while(i<=L.length&&*p!=cur_e)
  {/*?为什么前面使用函数指针,但是此却却直接使用值比较呢?*/
    p++;
    i++;
  }
  if(i>L.length)
    return INFEASIBLE; /* 操作失败 */
  else
  {
    *pre_e=*--p;
    return OK;
  }
}

Status NextElem(SqList L,ElemType cur_e,ElemType *next_e)
{ /* 初始条件：顺序线性表L已存在 */
  /* 操作结果：若cur_e是L的数据元素，且不是最后一个，则用next_e返回它的后继， */
  /*           否则操作失败，next_e无定义 */
  int i=1;
  ElemType *p=L.elem;
  while(i<L.length&&*p!=cur_e)
  {
    i++;
    p++;
  }
  if(i==L.length)
    return INFEASIBLE; /* 操作失败 */
  else
  {
    *next_e=*++p;
    return OK;
  }
}

Status ListInsert(SqList *L,int i,ElemType e)
{ /* 初始条件：顺序线性表L已存在，1≤i≤ListLength(L)+1 */
  /* 操作结果：在L中第i个位置之前插入新的数据元素e，L的长度加1 */
  ElemType *newbase,*q,*p;
  if(i<1||i>L->length+1) /* i值不合法 */
    return ERROR;
  if(L->length>=L->listsize) /* 当前存储空间已满,增加分配 */
  {
    newbase=realloc(L->elem,(L->listsize+LIST_INCREMENT)*sizeof(ElemType));
    if(!newbase)
      exit(OVERFLOW); /* 存储分配失败 */
    L->elem=newbase; /* 新基址 */
    L->listsize+=LIST_INCREMENT; /* 增加存储容量 */
  }
  q=L->elem+i-1; /* q为插入位置 */
  for(p=L->elem+L->length-1;p>=q;--p) /* 插入位置及之后的元素右移 */
    *(p+1)=*p;
  *q=e; /* 插入e */
  ++L->length; /* 表长增1 */
  return OK;
}

Status ListDelete(SqList *L,int i,ElemType *e)
{ /* 初始条件：顺序线性表L已存在，1≤i≤ListLength(L) */
  /* 操作结果：删除L的第i个数据元素，并用e返回其值，L的长度减1 */
  ElemType *p,*q;
  if(i<1||i>L->length) /* i值不合法 */
    return ERROR;
  p=L->elem+i-1; /* p为被删除元素的位置 */
  *e=*p; /* 被删除元素的值赋给e */
  q=L->elem+L->length-1; /* 表尾元素的位置 */
  for(++p;p<=q;++p) /* 被删除元素之后的元素左移 */
    *(p-1)=*p;
  L->length--; /* 表长减1 */
  return OK;
}

void ListTraverse(SqList L,void(*vi)(ElemType*))
{ /* 初始条件：顺序线性表L已存在 */
  /* 操作结果：依次对L的每个数据元素调用函数vi() */
  /*           vi()的形参加'&'，表明可通过调用vi()改变元素的值 */
  ElemType *p;
  int i;
  p=L.elem;
  for(i=1;i<=L.length;i++)
    vi(p++);
  printf("\n");
}
```

顺序表可以随机存取表中任一元素，缺点是插入删除时需要移动大量元素。链表则与其相反，无法随机存取，但可方便的进行插入和删除，链表一般可分为单链表、双链表和循环列表。

动态单链表及其操作如下所示：

```c
/*線性表的單鏈表存儲结構*/
typedef struct LNode{
  ElemType data;
  struct LNode *next;
}LNode, *LinkList;

/*带有头结點的單鏈表的基本操作(12个)*/
void InitList(LinkList *L)
{ /* 操作结果：構造一个空的線性表L */
  *L=(LinkList)malloc(sizeof(struct LNode)); /* 產生头结點，並使L指向此头结點 */
  if(!*L) /* 存儲分配失敗 */
    exit(OVERFLOW);
  (*L)->next=NULL; /* 指针域為空 */
}

void DestroyList(LinkList *L)
{ /* 初始條件：線性表L已存在。操作结果：销毁線性表L */
  LinkList q;
  while(*L)
  {q=(*L)->next;
    free(*L);
    *L=q;
  }
}

void ClearList(LinkList L) /* 不改变L */
{ /* 初始条件：线性表L已存在。操作结果：将L重置为空表 */
  LinkList p,q;
  p=L->next; /* p指向第一个结点 */
  while(p) /* 没到表尾 */
  {q=p->next;
    free(p);
    p=q;
  }
  L->next=NULL; /* 头结点指针域为空 */
}

Status ListEmpty(LinkList L)
{ /* 初始条件：线性表L已存在。操作结果：若L为空表，则返回TRUE，否则返回FALSE */
  return L->next == NULL;
}

int ListLength(LinkList L)
{ /* 初始条件：线性表L已存在。操作结果：返回L中数据元素个数 */
  int i=0;
  LinkList p=L->next; /* p指向第一个结点 */
  while(p) /* 没到表尾 */
  {
    i++;
    p=p->next;
  }
  return i;
}

Status GetElem(LinkList L,int i,ElemType *e) /* 算法2.8 */
{ /* L为带头结点的单链表的头指针。当第i个元素存在时，其值赋给e并返回OK，否则返回ERROR */
  int j=1; /* j为计数器 */
  LinkList p=L->next; /* p指向第一个结点 */
  while(p&&j < i) /* 顺指针向后查找，直到p指向第i个元素或p为空 */
  {
    p=p->next;
    j++;
  }
  if(!p||j>i) /* 第i个元素不存在 */
    return ERROR;
  *e=p->data; /* 取第i个元素 */
  return OK;
}

int LocateElem(LinkList L,ElemType e,Status(*compare)(ElemType,ElemType))
{ /* 初始条件: 线性表L已存在，compare()是数据元素判定函数(满足为1，否则为0) */
  /* 操作结果: 返回L中第1个与e满足关系compare()的数据元素的位序。 */
  /*           若这样的数据元素不存在，则返回值为0 */
  int i=0;
  LinkList p=L->next;
  while(p)
  {
    i++;
    if(compare(p->data,e)) /* 找到这样的数据元素 */
      return i;
    p=p->next;
  }
  return 0;
}

Status PriorElem(LinkList L,ElemType cur_e,ElemType *pre_e)
{ /* 初始条件: 线性表L已存在 */
  /* 操作结果: 若cur_e是L的数据元素，且不是第一个，则用pre_e返回它的前驱， */
  /*           返回OK；否则操作失败，pre_e无定义，返回INFEASIBLE */
  LinkList q,p=L->next; /* p指向第一个结点 */
  while(p->next) /* p所指结点有后继 */
  {
    q=p->next; /* q为p的后继 */
    if(q->data==cur_e)
    {
      *pre_e=p->data;
      return OK;
    }
    p=q; /* p向后移 */
  }
  return INFEASIBLE;
}

Status NextElem(LinkList L,ElemType cur_e,ElemType *next_e)
{ /* 初始条件：线性表L已存在 */
  /* 操作结果：若cur_e是L的数据元素，且不是最后一个，则用next_e返回它的后继， */
  /*           返回OK;否则操作失败，next_e无定义，返回INFEASIBLE */
  LinkList p=L->next; /* p指向第一个结点 */
  while(p->next) /* p所指结点有后继 */
  {
    if(p->data==cur_e)
    {
      *next_e=p->next->data;
      return OK;
    }
    p=p->next;
  }
  return INFEASIBLE;
}

Status ListInsert(LinkList L,int i,ElemType e) /* 算法2.9。不改变L */
{ /* 在带头结点的单链线性表L中第i个位置之前插入元素e */
  int j=0;
  LinkList p=L,s;
  while(p&&j < i-1) /* 寻找第i-1个结点 */
  {
    p=p->next;
    j++;
  }
  if(!p||j>i-1) /* i小于1或者大于表长 */
    return ERROR;
  s=(LinkList)malloc(sizeof(struct LNode)); /* 生成新结点 */
  s->data=e; /* 插入L中 */
  s->next=p->next;
  p->next=s;
  return OK;
}

Status ListDelete(LinkList L,int i,ElemType *e) /* 算法2.10。不改变L */
{ /* 在带头结点的单链线性表L中，删除第i个元素，并由e返回其值 */
  int j=0;
  LinkList p=L,q;
  while(p->next&&j< i-1) /* 寻找第i个结点，并令p指向其前岖 */
  {
    p=p->next;
    j++;
  }
  if(!p->next||j>i-1) /* 删除位置不合理 */
    return ERROR;
  q=p->next; /* 删除并释放结点 */
  p->next=q->next;
  *e=q->data;
  free(q);
  return OK;
}

void ListTraverse(LinkList L,void(*vi)(ElemType))
/* vi的形参类型为ElemType，与bo2-1.c中相应函数的形参类型ElemType&不同 */
{ /* 初始条件：线性表L已存在。操作结果：依次对L的每个数据元素调用函数vi() */
  LinkList p=L->next;
  while(p)
  {
    vi(p->data);
    p=p->next;
  }
  printf("\n");
}
```