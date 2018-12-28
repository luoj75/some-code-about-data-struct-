数据结构-排序-查找-二叉树

# 1. 查找


  * 顺序查找

---
  * 二分查找  

 *有序表*
 * 健忘版
 ```cpp
 int binary_search1(list, start, end, key)
 {
     if(start < end )
     {
         int mid = (start + end ) / 2 ; 
         if( key > list[mid] ) return binary_search1(list, mid+1, end, key);
         else return binary_search1(list, start, mid, key);
     }
     else if(start > end ) return -1 
     else {
         if(list[start] == key) return start
         else return -1
     }
 }
 ```
 * 识别相等版
 ```cpp
 int binary_search2(list, start, end, key)
 {
     if(start <= end )
     {
         int mid = (start +end ) /2 ;
         if(key == list[mid]) return mid ; 
         else if(key > list[mid]) return binary_search2(list, mid+1, end, key) ; 
         else return binary_search2(list, start, mid-1, key) ; 
     }
     else  return -1;
 }
```

# 2. 排序

## 2.1. 插入排序  

---
```cpp
void insert_sort(list)
{
    for(int i=1; i<list.size(); i++)
    {
        int temp = list[i] ;
        int j;
        for( j=i; j>0&&temp<list[j-1]; j--) list[j] = list[j-1];
        list[j] = temp ;
    }
}
```
**测试**
链式版本
```cpp
template<typename T>
struct Node
{   T entry;
    Node *next;
    Node(T input, Node *p=NULL){enrty=input; next=p;}
};
void insert_sort(head)
{
    Node<int> *first_unsorted, *last_sorted, *current, *trailing ; 
    if(head != NULL)
    {
       last_sorted = head ; 
       while(list_sorted->next != NULL)
       {
           first_unsorted = last_sorted->next;
           if(first_unsorted->entry < head->entry){
              last_sorted->next = first_unsorted->next;
              first_unsorted->next = head 
              head = first_unsorted ;
           }
           else{
            trailing = head ;
            current = trailing->next;
            while(first_unsorted->entry > current->entry)
            {
                trailing = current;
                current =  trailing->next;
            }
            if(first_unsorted == current) last_sorted = first_unsorted ;
            else{
                last_sorted->next = first_unsorted->next;
                fisrt_unsorted->next = current 
                trailing->next = first_unsorted
            }
           }
       }
    }
}
```

## 2.2. 选择排序
```cpp
void select_sort(int *list, int count)
{ 
    int min;    //将未排序部分的最大或者最小值，与当前位置的值交换，不断扩大已排序部分，直到完成
    int min_indx;  
    for (int i = 0; i < count; i ++) {
        min = list[i] ;
        min_index = i;
        for (int j = i; j < count; j ++) {
            if (min > list[j]) {
                min = list[j];
                min_index = j;
            }
        }
        list[min_index] = list[i];
        list[i] = min;
    }
}
```

## 2.3. 希尔排序  

**二路希尔排序**
```cpp
void shellSort(int *a,int len) //希尔排序,len：数组长度
{
    int i,j,increment,temp;
    for(increment=len/2;increment>0;increment/=2)   //二路希尔排序
    {
        for(i=increment;i<len;i++)   //进行插入排序
        {
            temp=a[i];
            for(j=i; j>=increment&&a[j-increment]>temp; j-=increment)a[j]=a[j-increment] ;   //升序
            a[j]=temp;
        }
    }
}
```

**任意步长希尔排序**
```cpp
void shellSort(int *a,int len, int step) //希尔排序,len：数组长度，step:步长
{
    int i,j,increment,temp;
    for(increment=len-step; increment>0; increment -= step)   //二路希尔排序
    {
        for(i=increment;i<len;i++)   //进行插入排序
        {
            temp=a[i];
            for(j=i; j>=increment&&a[j-increment]>temp; j-=increment)a[j]=a[j-increment] ;   //升序
            a[j]=temp;
        }
    }
}
```

## 2.4. 归并排序  
```cpp
void Merge(int *arr, int n)
{
    int temp[n];// 辅助数组
    int b = 0;// 辅助数组的起始位置
    int mid = n / 2;// mid将数组从中间划分，前一半有序，后一半有序
    int first = 0, second = mid;// 两个有序序列的起始位置

    while (first < mid && second < n)
    {
        if (arr[first] <= arr[second])// 比较两个序列
            temp[b++] = arr[first++];
        else
            temp[b++] = arr[second++];
    }

    while(first < mid)// 将剩余子序列复制到辅助序列中
            temp[b++] = arr[first++];
    while(second < n)
            temp[b++] = arr[second++];
    for(int i=0; i < n; i++)//辅助序列复制到原序列
        arr[i] = temp[i];
}

void MergeSort(int *arr, int n)
{
    if(n<=1)
        return;
    if (n > 1)
    {
        MergeSort(arr, n / 2);      // 对前半部分进行归并排序
        MergeSort(arr + n / 2, n - n / 2);// 对后半部分进行归并排序
        Merge(arr, n); // 归并两部分
    }
}
```

## 2.5. 快速排序

```c++
int OnceSort(int arr[], int first, int end)
{
 int i = first,j = end;    //当i<j即移动的点还没到中间时循环
 while(i < j)
 {     
  while(i < j && arr[i] <= arr[j]) --j;    //右边区开始，保证i<j并且arr[i]小于或者等于arr[j]的时候就向左遍历
  if(i < j)swap(arr[i],arr[j]); //这时候已经跳出循环，说明j>i 或者 arr[i]大于arr[j]了，如果i<j那就是arr[i]大于arr[j]，那就交换

  while(i < j && arr[i] <= arr[j]) ++i;  //对另一边执行同样的操作
  if(i < j)swap(arr[i],arr[j]);
 }
 return i;  //返回已经移动的一边当做下次排序的轴值
}

void QuickSort(int arr[], int first, int end){
 if (first < end)
{
   int pivot = OnceSort(arr,first,end); //已经有轴值了，再对轴值左右进行递归
   QuickSort(arr,first,pivot-1);
   QuickSort(arr,pivot+1,end);
}
}
```

## 2.6. 堆排序
```cpp
void Heapify(int arr[], int first, int end){
    int father = first;
    int son = father * 2 + 1;
    while(son < end){
        if(son + 1 < end && arr[son] < arr[son+1]) ++son;
        if(arr[father] > arr[son]) break; //如果父节点大于子节点则表示调整完毕
        else {              //不然就交换父节点和子节点的元素
            swap(arr[father],arr[son])
            father = son;  //父和子节点变成下一个要比较的位置
            son = 2 * father + 1;
        }
    }
}
void HeapSort(int arr[],int len){
    int i;
    for(i = len/2 - 1; i >= 0; --i){  //初始化堆，从最后一个父节点开始
        Heapify(arr,i,len);
    }
    for(i = len - 1;i > 0;--i){     //从堆中的取出最大的元素再调整堆
        int temp = arr[i];
        arr[i] = arr[0];
        arr[0] = temp;
        Heapify(arr,0,i);        //调整成堆
    }
}
```
## 2.7. 测试
```cpp
int main()
{
    int a[9]={3,1,5,7,2,4,4,9,6};
    HeapSort(a,9);
    print(a,9);
}
```

# 3. 哈希表


# 4. 二叉树

```cpp
template<typename T> 
struct Binary_node{
    T  data;
    Binary_node<T> *left;
    Binary_node<T> *right;
    Binary_node();
    Binary_node(T &x, Binary_node<T> *l , Binary_node<T> *r)
    { data=x; left=l; right=r; }
}
```
## 4.1. 二叉树遍历
### 4.1.1. 前序遍历
```cpp
void recursive_preorder(Binary_node<T> *root)
{
    if(root!=NULL)
    {
        cout<<root->data<<endl;
        recursive_preorder(root->left);
        recursive_preorder(root->right);
    }
}
```
### 4.1.2. 中序遍历
### 4.1.3. 后序遍历

## 4.2. Binary_tree类实现
```cpp
template<typename T>
class Binary_tree{
private:
    Binary_node<T> *root;
public:
    Binary_tree();
    Binary_tree(const Binary_tree<T>& original);
    Binary_tree & operator=(const Binary_tree<T>& original);
    bool empty();
    void preorder(void(*visit)(T &));
    void inorder(void(*visit)(T &));
    void posorder(void(*visit)(T &));
    int size()const;
    void clear();
    void remove(const T &x);
    void insert(const T &x);
    void find(const T &x);
    void DFS();
    void BFS();
    int height()const
    {
        if(root!=NULL)
       {
            int m = height ( root->left);
            int n = height(root->right);
            return (m > n) ? (m+1) : (n+1);
       }
       else return -1;
    }
    int width(const treeNode * root)//所有深度中含有的最多的子叶数。最宽的那一层
    {
        if(root == NULL) return 0;

        int Width = 1;   
        int LastLevelWidth = 0;  //记录上一层的宽度
        int TempLastLevelWidth = 0;
        int CurLevelWidth = 0;  //当前层的宽度

        queue<treeNode *> myQueue;
        treeNode *tree = new treeNode(0,root->left,root->right);
        myQueue.push(tree);
        LastLevelWidth = 1;
        treeNode *Cur = NULL;

        while(!myQueue.empty())
        {
            TempLastLevelWidth = LastLevelWidth;
            while(TempLastLevelWidth != 0 )
            {
                Cur = myQueue.front();
                myQueue.pop();
                if(Cur->left) myQueue.push(Cur->left);
                if(Cur->right) myQueue.push(Cur->right);
                TempLastLevelWidth--;
            }
            CurLevelWidth = myQueue.size();
            Width = CurLevelWidth > Width ? CurLevelWidth : Width;
            LastLevelWidth = CurLevelWidth;
        }
        return Width;
    }
}
```

## 4.3. 二叉查找树
```cpp
struct BinaryNode
{
  int elem;
  BinaryNode *left;
  BinaryNode * right;
  BinaryNode( int d,BinaryNode *l=NULL, BinaryNode *r=NULL):elem(d),left(l),right(r){};
};

void insertion(BinaryNode *root , int x)
{
    BinaryNode *p=new BinaryNode(x,NULL,NULL);

    if(root == NULL)root = p;
    else if(x <= root->elem) insertion(root->left, x);
    else if(x > root->elem) insertion(root->right, x);
    else {//重复的数据不添加到树中}
}

void remove(BinaryNode *root , int x)
{
    if(root==NULL)return ;
    else{
        if(x > root->elem) remove(root->right, x);
        else if(x < root->elem) remove(root->left,x);
        else if(root->left!=NULL && root->right!=NULL)//有左右子节点，将右子树中最小点覆盖到当前节点，删除右子树中的该结点
        {
            root->elem = findMin(root->right)->elem;
            remove(root->right,root->elem);
        }
        else
        {
            BianryNode *oldNode = root;
            root = (root->left != NULL)?root->left:root->right;
            delete oldNode;
        }
    }
}
BinaryNode * findMin(BinaryNode *bNode) const 
{
    if ( nullptr!= bNode) {
        while( nullptr != bNode->leftNode) {
            bNode = bNode->leftNode;
        }
    }
    return bNode;
}

BinaryNode* creatBinarySearchTree(int len, int ptr[])
{
    BinaryNode *p=new BinaryNode(ptr[0],NULL,NULL);

	for (int i = 1; i < len; i++)insertion(p , ptr[i]);
	return p;
}

bool binary_search(BinaryNode *root, int key)
{
    if(root==NULL)return false;
    else if(key == root->elem)return true;
    else if(key > root->elem) return b_s(root->right,key);
    else return b_s(root->left,key);
}
```

## 4.4. DFS/BFS

```cpp
template <typename T>
struct BinaryNode{
  T elem;
  BinaryNode *left;
  BinaryNode * right;
  BinaryNode(T d, BinaryNode *l=NULL, BinaryNode *r=NULL):elem(d),left(l),right(r){};
};

void DFS(BinaryNode<T> *root)
{
     if(root)
     {
         if(root->left)DFS(root->left);
         if(root->right)DFS(root->right);
         cout<<root->elem<<" "<<endl;
     }
}

void DFS3(BinaryNode<T> *root)
{
    stack<BinaryNode<T> *> s;
    s.push(root);
    while(!s.empty())
    {
        BinaryNode<T> temp=s.top();
        cout<<temp->elem<<endl;
        s.pop();
        if(temp->right)s.push(temp->right);
        if(temp->left)s.push(temp->left);
    }
}


void BFS(BinaryNode<T> *root)
{
    queue<BinaryNode<T> *> q;
    q.push(root);
    BinaryNode<T> *p=root;
    while(!q.empty())
    {
        BinaryNode<T> temp=q.front();
        cout<<temp->elem<<endl;
        q.pop();
        if(p->left)q.push(p->left);
        if(p->right)q.push(p->right);
    }
}

template <typename T>
void levelTraversal(BinaryNode<T>* root, void (*visit)(T &x))
{
    if(root==NULL)return 0;
    queue<BinaryNode<T>* > nodeQueue;  //使用C++的STL标准模板库
    nodeQueue.push(root);
    while(!nodeQueue.empty()){
        BinaryNode<T>* node;
        node = nodeQueue.front();
        nodeQueue.pop();
        visit(node->elem);
        if(node->left)nodeQueue.push(node->left);  //先将左子树入队
        if(node->right)nodeQueue.push(node->right);  //再将右子树入队
    }
}
```


# 5. 图

## 5.1. 图的遍历


## 5.2. 最短路径Dijstra算法


