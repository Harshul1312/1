# 1#include <stdio.h>
#include <stdlib.h>

// Node structure for linked list
struct Node {
    int data;
    struct Node* next;
};

// Function to create a new node with given data
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Function to insert a new node at the end of the list
void append(struct Node** head, int data) {
    struct Node* newNode = createNode(data);
    if (*head == NULL) {
        *head = newNode;
        return;
    }
    
    struct Node* temp = *head;
    while (temp->next != NULL) {
        temp = temp->next;
    }
    temp->next = newNode;
}

// Function to print the linked list
void printList(struct Node* head) {
    struct Node* temp = head;
    while (temp != NULL) {
        printf("%d -> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}

// Merge two sorted linked lists
struct Node* merge(struct Node* left, struct Node* right) {
    if (left == NULL) return right;
    if (right == NULL) return left;
    
    struct Node* result = NULL;
    
    if (left->data <= right->data) {
        result = left;
        result->next = merge(left->next, right);
    } else {
        result = right;
        result->next = merge(left, right->next);
    }
    
    return result;
}

// Function to split the linked list into two halves
void splitList(struct Node* head, struct Node** front, struct Node** back) {
    struct Node* slow = head;
    struct Node* fast = head->next;
    
    while (fast != NULL) {
        fast = fast->next;
        if (fast != NULL) {
            slow = slow->next;
            fast = fast->next;
        }
    }
    
    *front = head;
    *back = slow->next;
    slow->next = NULL;
}

// Merge Sort function for linked list
struct Node* mergeSort(struct Node* head) {
    if (head == NULL || head->next == NULL)
        return head;
    
    struct Node* left;
    struct Node* right;
    
    // Split the list into two halves
    splitList(head, &left, &right);
    
    // Recursively sort the two halves
    left = mergeSort(left);
    right = mergeSort(right);
    
    // Merge the sorted halves
    return merge(left, right);
}

int main() {
    struct Node* head = NULL;
    int n, value;
    
    // Input the number of elements
    printf("Enter the number of elements: ");
    scanf("%d", &n);
    
    // Input the elements and append them to the list
    for (int i = 0; i < n; i++) {
        printf("Enter element %d: ", i + 1);
        scanf("%d", &value);
        append(&head, value);
    }
    
    printf("\nOriginal List: \n");
    printList(head);
    
    // Sort the list using merge sort
    head = mergeSort(head);
    
    printf("\nSorted List: \n");
    printList(head);
    
    return 0;
}
