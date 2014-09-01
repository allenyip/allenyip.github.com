---

title: 二叉搜索树与红黑树

layout: post

---

##**二叉搜索树**

二叉搜索树（Binary Search Tree），或者是一棵空树，或者是具有下列性质的二叉树： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为二叉搜索树。

```c
INORDER_TREE_WALK(x)
/* 中序遍历 */
if x != NIL
  INORDER_TREE_WALK(x.left)
  print x.key
  INORDER_TREE_WALK(x.right)
```

```c
INORDER_TREE_WALK(x)
/* 非递归中序遍历 */
while x != NIL or !tempStack.empty
  while x != NIL
    tempStack.push(x)
    x = x.left
  if !tempStack.empty
    print tempStack.pop
    x = x.right
```

```c
TREE_SEARCH(x, k)
/* 查找 */
if x == NIL or k == x.key
  return x
if k < x.key
  TREE_SEARCH(x.left, k)
else TREE_SEARCH(x.right, k)
```

```c
TREE_SEARCH2(x, k)
/* while循环查找，效率更高 */
while x != NIL and x.key != k
  if k < x.key
    x = x.left
  else x = x.right
return x
```

```c
TREE_MINIMUM(x)
/* 最小值 */
while x.left != NIL
  x = x.left
return x

TREE_MAXIMUM(x)
/* 最小值 */
while x.right != NIL
  x = x.right
return x
```

```c
TREE_SUCCESSOR(x)
/* 后继 */
/* 有右子数，则后继为右子树的最小值 */
if x.right != NIL
  return TREE_MINIMUN(x.right)
/* 无右子树，则后继为最近的（其左子树也为祖先的）祖先 */
y = x.p
while y !=NIL and x == y.right
  x = y
  y = y.p
return y
```

```c
TREE_INSERT(T, z)
/* 插入 */
while x != NIL
  y = x /* 遍历指针y */
  if z.key < x.key
    x = x.left
  else x = x.right
z.p = y
if y == NIL /* T为空 */
  T.root = z
elseif z.key < y.key
  y.left = z
else y.right = z
```

```c
TREE_DELETE(T, z)
/* 删除 */
if z.left == NIL /* 没左孩子，不管是否有右孩子 */
  TRANSPLANT(T, z, z.right)
elseif z.right == NIL /* 有左孩子没右孩子 */
  TRANSPLANT(T, z, z.left)
else /* 有左右孩子 */
  y = TREE_MINIMUM(z.right)/* 找到后继 */
  if y.p != z /* 后继不是右孩子 */
    TRANSPLANT(T, y, y.right)
    y.right = z.right
    y.right.p = y
  TRANSPLANT(T, z, y)
  y.left = z.left
  y.left.p = y

TRANSPLANT(T, u, v)
/* 以v为根的子树替换以u为根的子树(未处理v.left和v.right) */
if u.p == NIL
  T.root = v
elseif u == u.p.left
  u.p.left = v
else u.p.right = v
if v != NIL
  v.p = u.p
```

##**红黑树**

二叉搜索树的操作时间复杂度都不超过O(h)，其中h红黑树则为O(lgn)。

红黑树是每个节点都带有颜色属性的二叉搜索树，颜色或红色或黑色，并且具有如下性质：

1，每个节点是红色或黑色。

2，根节点是黑色。

3，每个叶节点（NIL节点，空节点）是黑色的。

4，每个红色节点的两个子节点都是黑色。(从每个叶子到根的所有路径上不能有两个连续的红色节点)

5，从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。

```c
LEFT_ROTATE(T, x)
/* 左旋 */
y = x.right
x.right = y.left
if x.left != T.NIL
  y.left.p = x
y.p = x.p
if x.p == T.NIL
  T.root = y
elseif x = x.p.left
  x.p.left = y
else x.p.right = y
y.left = x
x.p = y
```

```c
RB_INSERT(T, z)
/* 插入 */
y = T.NIL
x = T.root
while x != T.NIL
  y = x
  if z.key < x.key
    x = x.left
  else x = x.right
z.p = y
if y == T.NIL
  T.root = z
elseif z.key < y.key
  y.left = z
else y.right = z
z.left = T.NIL
z.right = T.NIL
z.color = RED
RB_INSERT_FIXUP(T, z)

RB_INSERT_FIXUP(T, z)
/* 保持红黑树性质 */
while z.p.color == RED
  if z.p == z.p.p.left
    y = z.p.p.right
    if y.color == RED
      z.p.color = BLACK                   //Case 1
      y.color = BLACK                     //Case 1
      z.p.p.color = RED                   //Case 1
      z = z.p.p                           //Case 1
    elseif z == z.p.right
      z = z.p                             //Case 2
      LEFT_ROTATE(T, z)                   //Case 2
    z.p.color = BLACK                     //Case 3
    z.p.p.color = RED                     //Case 3
    RIGHT_ROTATE(T, z.p.p)                //Case 3
  else (same as then clause
        with "right" and "left" exchanged)
T.root.color = BLACK
```

```c
RB_DELETE(T, z)
/* 删除 */
y = z
y-original-color = y.color
if z.left == T.NIL /* 没左孩子，不管是否有右孩子 */
  x = z.right
  RB_TRANSPLANT(T, z, z.right)
elseif z.right == T.NIL /* 有左孩子没右孩子 */
  x = z.left
  RB_TRANSPLANT(T, z, z.left)
else /* 有左右孩子 */
  y = TREE_MINIMUM(z.right)/* 找到后继 */
  y-original-color = y.color
  x = y.right
  if y.p == z
    x.p = y
  else
    RB_TRANSPLANT(T, y, y.right)
    y.right = z.right
    y.right.p = y
  RB_TRANSPLANT(T, z, y)
  y.left = z.left
  y.left.p = y
  y.color = z.color
if y.original-color == BLACK
  RB_DELETE_FIXUP(T, x)

RB_TRANSPLANT(T, u, v)
/* 以v为根的子树替换以u为根的子树(未处理v.left和v.right) */
if u.p == T.NIL
  T.root = v
elseif u == u.p.left
  u.p.left = v
else u.p.right = v
v.p = u.p

RB_DELETE_FIXUP(T, z)
/* 保持红黑树性质 */
 1 while color[p[z]] = RED
 2     do if p[z] = left[p[p[z]]]
 3           then y ← right[p[p[z]]]
 4                if color[y] = RED
 5                   then color[p[z]] ← BLACK                   //Case 1
 6                        color[y] ← BLACK                      //Case 1
 7                        color[p[p[z]]] ← RED                  //Case 1
 8                        z ← p[p[z]]                           //Case 1
 9                   else if z = right[p[z]]
10                           then z ← p[z]                      //Case 2
11                                LEFT-ROTATE(T, z)             //Case 2
12                           color[p[z]] ← BLACK                //Case 3
13                           color[p[p[z]]] ← RED               //Case 3
14                           RIGHT-ROTATE(T, p[p[z]])           //Case 3
15           else (same as then clause
                         with "right" and "left" exchanged)
16 color[root[T]] ← BLACK
```