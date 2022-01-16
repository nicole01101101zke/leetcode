1066 Root of AVL Tree (25 分)

An AVL tree is a self-balancing binary search tree. In an AVL tree, the heights of the two child subtrees of any node differ by at most one; if at any time they differ by more than one, rebalancing is done to restore this property. Figures 1-4 illustrate the rotation rules.

![F1.jpg](https://images.ptausercontent.com/8e3c8cca-d5ab-490b-be8b-c7101ffb94a4.jpg) ![F2.jpg](https://images.ptausercontent.com/bcdb39fb-08b6-41d8-8a3d-96708e4ad97c.jpg)

![img](https://images.ptausercontent.com/33) ![img](https://images.ptausercontent.com/34)

Now given a sequence of insertions, you are supposed to tell the root of the resulting AVL tree.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤20) which is the total number of keys to be inserted. Then *N* distinct integer keys are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the root of the resulting AVL tree in one line.

### Sample Input 1:

```in
5
88 70 61 96 120结尾无空行
```

### Sample Output 1:

```out
70
```

### Sample Input 2:

```
7
88 70 61 96 120 90 65
```

### Sample Output 2:

```
88
```

```c++
#include <iostream>
#include <algorithm>

using namespace std;

struct Node{
    int val,lheight,rheight;
    Node *right,*left;
};

void insert(Node*&root,int i){
    if(root==NULL){
            root = new Node;
            root->val = i;
            root->right=root->left = NULL;
            root->lheight = root->rheight = 0;
        }else{
            if(i>root->val){
                insert(root->right,i);
                root->rheight = max(root->right->lheight,root->right->rheight)+1;
            }else{
                insert(root->left,i);
                root->lheight = max(root->left->lheight,root->left->rheight)+1;
            }//插入节点和高度更新

            if(root->lheight-root->rheight==2){
                if(root->left->lheight>root->left->rheight){//左左
                    Node* a = root;
                    Node* b = root->left;
                    root=b;
                    a->left=b->right;
                    a->lheight = b->rheight;
                    b->right=a;
                    b->rheight++;
                }else{//左右
                    Node* a = root;
                    Node* b = root->left;
                    Node* c = root->left->right;
                    root=c;
                    b->right=c->left;
                    b->rheight = c->lheight;
                    a->left=c->right;
                    a->lheight = c->rheight;
                    c->left=b;
                    c->right=a;
                    c->lheight = b->lheight+1;
                    c->rheight = a->rheight +1;
                }
            }else if(root->lheight-root->rheight==-2){
                if(root->right->rheight>root->right->lheight){//右右
                    Node* a = root;
                    Node* b = root->right;
                    root=b;
                    a->right=b->left;
                    a->rheight = b->lheight;
                    b->left=a;
                    b->lheight++;
                }else{//左右
                    Node* a = root;
                    Node* b = root->right;
                    Node* c = root->right->left;
                    root=c;
                    b->left=c->right;
                    b->lheight = c->rheight;
                    a->right=c->left;
                    a->rheight = c->lheight;
                    c->right=b;
                    c->left=a;
                    c->lheight = a->lheight+1;
                    c->rheight = b->rheight +1;
                }
            }
        }
}
int main(){
    int N,i;
    Node* tree=NULL;
    cin>>N;
    while(N--){
        cin>>i;
        insert(tree,i);
    }
    cout<<tree->val;
}
```

