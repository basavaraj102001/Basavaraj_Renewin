# Basavaraj_Renewin
//Complete Implementation Code 


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Creating Structure for directory node
typedef struct directory {
    char name[50];
    struct directory *left;
    struct directory *right;
} Directory;

// Function to insert a new directory 
Directory* insert(Directory* root, char name[]) {
    if (root == NULL) {
        Directory* newNode = (Directory*) malloc(sizeof(Directory));
        strcpy(newNode->name, name);
        newNode->left = newNode->right = NULL;
        return newNode;
    }
    if (strcmp(name, root->name) < 0)
        root->left = insert(root->left, name);
    else if (strcmp(name, root->name) > 0)
        root->right = insert(root->right, name);
    return root;
}

// Function to delete a directory 
Directory* delete(Directory* root, char name[]) {
    if (root == NULL)
        return root;
    if (strcmp(name, root->name) < 0)
        root->left = delete(root->left, name);
    else if (strcmp(name, root->name) > 0)
        root->right = delete(root->right, name);
    else {
        // Case 1: No child or 1 child (Base Case)
        if (root->left == NULL) {
            Directory* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            Directory* temp = root->left;
            free(root);
            return temp;
        }
        // Case 2: 2 children
        Directory* temp = root->right;
        while (temp->left != NULL)
            temp = temp->left;
        strcpy(root->name, temp->name);
        root->right = delete(root->right, temp->name);
    }
    return root;
}

// Function to search for a directory 
Directory* search(Directory* root, char name[]) {
    if (root == NULL || strcmp(name, root->name) == 0)
        return root;
    if (strcmp(name, root->name) < 0)
        return search(root->left, name);
    return search(root->right, name);
}

// Function to traverse the directory structure in pre-order
void preOrder(Directory* root) {
    if (root != NULL) {
        printf("%s\n", root->name);
        preOrder(root->left);
        preOrder(root->right);
    }
}

// Function to traverse the directory structure in-order
void inOrder(Directory* root) {
    if (root != NULL) {
        inOrder(root->left);
        printf("%s\n", root->name);
        inOrder(root->right);
    }
}

// Function to traverse the directory structure in post-order
void postOrder(Directory* root) {
    if (root != NULL) {
        postOrder(root->left);
        postOrder(root->right);
        printf("%s\n", root->name);
    }
}

// Function to print the directory structure in a formatted way
void print(Directory* root, int level) {
    if (root == NULL)
        return;
    for (int i = 0; i < level; i++)
        printf("  ");
    printf("%s\n", root->name);
    print(root->left, level + 1);
    print(root->right, level + 1);
}

int main() {
    Directory* root = NULL;
    int choice;
    char name[50];
        while (1) {
        printf("\nDirectory Operations:\n");
        printf("1. Insert\n");
        printf("2. Delete\n");
        printf("3. Search\n");
        printf("4. Pre-Order Traversal\n");
        printf("5. In-Order Traversal\n");
        printf("6. Post-Order Traversal\n");
        printf("7. Print\n");
        printf("8. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1:
                printf("Enter directory/file name to insert: ");
                scanf("%s", name);
                root = insert(root, name);
                break;
            case 2:
                printf("Enter directory/file name to delete: ");
                scanf("%s", name);
                root = delete(root, name);
                break;
            case 3:
                printf("Enter directory/file name to search: ");
                scanf("%s", name);
                Directory* result = search(root, name);
                if (result == NULL)
                    printf("Directory/file not found.\n");
                else
                    printf("Directory/file found: %s\n", result->name);
                break;
            case 4:
                printf("Pre-Order Traversal:\n");
                preOrder(root);
                break;
            case 5:
                printf("In-Order Traversal:\n");
                inOrder(root);
                break;
            case 6:
                printf("Post-Order Traversal:\n");
                postOrder(root);
                break;
            case 7:
                printf("Directory Structure:\n");
                print(root, 0);
                break;
            case 8:
                printf("Exiting...\n");
                exit(0);
            default:
                printf("Invalid choice. Try again.\n");
                break;
        }
    }
    return 0;
}

