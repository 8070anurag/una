# una

#include <bits/stdc++.h>
using namespace std;

class node
{
    public:
    int data;
    node *left;
    node *right;
    bool lThread, rThread;

    node()
    {
        left = right = nullptr;
        lThread = rThread = true;
    }
    node(int val)
    {
        data = val;
        left = right = nullptr;
        lThread = rThread = true;
    }
};





void insertIntoBST(node *&root, int key)
{
    node *newNode = new node(key);

    if (root == NULL)
    {
        root = newNode;
        return;
    }

    node *temp = root;

    while (true)
    {
        if (temp->data > key)
        {
            if (temp->left == NULL)
            {
                temp->left = newNode;
                return;
            }
            else
                temp = temp->left;
        }
        else
        {
            if (temp->right == NULL)
            {
                temp->right = newNode;
                return;
            }
            else
                temp = temp->right;
        }
    }
}





void printInorder(node *&root)
{
    if (root == NULL)
        return;

    printInorder(root->left);
    cout << root->data << " ";
    printInorder(root->right);
}







void insert_Node(int key, node *&root)
{
    node *p = new node(key);

    if (root->left == NULL)
    {
        root->left = p;
        p->left = root;
        p->right = root;
        root->lThread = false;
        return;
    }
    else
    {
        node *temp = root->left;

        while (true)
        {
            if (key < temp->data)
            {
                if (temp->lThread == true)
                {
                    p->left = temp->left;
                    temp->left = p;
                    temp->lThread = false;
                    p->right = temp;
                    return;
                }
                else
                {
                    temp = temp->left;
                }
            }
            else
            {
                if (temp->rThread == true)
                {
                    p->right = temp->right;
                    temp->right = p;
                    temp->rThread = false;
                    p->left = temp;
                    return;
                }
                else
                {
                    temp = temp->right;
                }
            }
        }
    }
}











void BSTIntoTBT(node *root, vector<node> v)
{
    for (int i = 0; i < v.size(); i++)
    {
        if (i != 0 && v[i].left == nullptr)
        {
            v[i].left = &v[i - 1];
        }
        else
        {
            v[i].lThread = false;
        }

        if (i != v.size() - 1 && v[i].right == nullptr)
        {
            v[i].right = &v[i + 1];
        }
        else
        {
            v[i].rThread = false;
        }

    }
}



void InorderSucc(node *root, vector<node> &v)
{
    if (root == NULL)
    {
        return;
    }

    InorderSucc(root->left, v);
    v.push_back(*root);
    InorderSucc(root->right, v);
}






void inOrderTraversal(node *root)
{

    node *current = root->left;

    while (current != root)
    {

        while (current->lThread == false)
        {
            current = current->left;
        }

        cout << current->data << " ";
    

        if (current->rThread == false)
        {
            current = current->right;
        }
        else
        {

            while (current->rThread == true && current->right != root)
            {
                current = current->right;
                cout << current->data << " ";
               
            }
            current = current->right;
        }
    }
    cout << endl;
}

void preorderTraversal(node *root)
{
    node *current = root->left;

    while (current != root)
    {
        cout << current->data << " ";

        if (current->lThread == false)
        {
            current = current->left;
        }
        else if (current->rThread == false)
        {
            current = current->right;
        }
        else
        {
            while (current->rThread == true && current->right != root)
            {
                current = current->right;
            }
            current = current->right;
        }
    }
    cout << endl;
}


int main()
{
    node *dummy = new node(999);

    insert_Node(6, dummy);
    insert_Node(3, dummy);
    insert_Node(8, dummy);
    insert_Node(1, dummy);
    insert_Node(5, dummy);
    insert_Node(7, dummy);
    insert_Node(11, dummy);
    insert_Node(9, dummy);
    insert_Node(13, dummy);

    cout << "Inorder: ";
    inOrderTraversal(dummy);

    cout << "Preorder: ";
    preorderTraversal(dummy);






    node *bst = nullptr;
    insertIntoBST(bst, 6);
    insertIntoBST(bst, 3);
    insertIntoBST(bst, 8);
    insertIntoBST(bst, 1);
    insertIntoBST(bst, 5);
    insertIntoBST(bst, 7);
    insertIntoBST(bst, 11);
    insertIntoBST(bst, 9);
    insertIntoBST(bst, 13);

    vector<node> v;

    InorderSucc(bst, v);

    int n = v.size();
   
    BSTIntoTBT(bst, v);

    v[0].left = NULL;
    v[0].lThread = true;

    v[n - 1].right = NULL;
    v[n - 1].rThread = true;



    printInorder(bst);

    return 0;

    
}
