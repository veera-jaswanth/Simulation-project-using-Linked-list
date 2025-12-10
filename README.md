#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};

struct Node *head = NULL;
int size = 0;

struct Node* createNode(int data) {
    struct Node *newNode = (struct Node*)malloc(sizeof(struct Node));
    if (newNode == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    printf("Created node @%p with data %d\n", (void*)newNode, data);
    return newNode;
}

void insertFront(int data) {
    struct Node *newNode = createNode(data);
    newNode->next = head;
    head = newNode;
    size++;
    printf("Inserted %d at FRONT\n", data);
}

void insertEnd(int data) {
    struct Node *newNode = createNode(data);
    if (head == NULL) {
        head = newNode;
    } else {
        struct Node *temp = head;
        while (temp->next != NULL)
            temp = temp->next;
        temp->next = newNode;
    }
    size++;
    printf("Inserted %d at END\n", data);
}

void insertAfter(int key, int data) {
    struct Node *temp = head;
    while (temp != NULL && temp->data != key)
        temp = temp->next;

    if (temp == NULL) {
        printf("Key %d not found\n", key);
        return;
    }

    struct Node *newNode = createNode(data);
    newNode->next = temp->next;
    temp->next = newNode;
    size++;
    printf("Inserted %d after %d\n", data, key);
}

void deleteNode(int key) {
    if (head == NULL) {
        printf("List is empty\n");
        return;
    }

    struct Node *temp = head;
    struct Node *prev = NULL;

    while (temp != NULL && temp->data != key) {
        prev = temp;
        temp = temp->next;
    }

    if (temp == NULL) {
        printf("Key %d not found\n", key);
        return;
    }

    if (prev == NULL) {           // deleting head
        head = head->next;
    } else {
        prev->next = temp->next;
    }

    printf("Deleted node %d @%p\n", temp->data, (void*)temp);
    free(temp);
    size--;
}

void display() {
    printf("\n=== LINKED LIST (size=%d) ===\n", size);
    if (head == NULL) {
        printf("EMPTY\n\n");
        return;
    }
    struct Node *temp = head;
    printf("HEAD @%p\n", (void*)head);
    while (temp != NULL) {
        printf("[%d] @%p -> %p\n", temp->data, (void*)temp, (void*)temp->next);
        temp = temp->next;
    }
    printf("NULL\n\n");
}

void freeAll() {
    struct Node *temp = head;
    while (temp != NULL) {
        struct Node *next = temp->next;
        free(temp);
        temp = next;
    }
    head = NULL;
    size = 0;
}

int main() {
    int choice, data, key;

    while (1) {
        printf("1.Insert Front  2.Insert End  3.Insert After  ");
        printf("4.Delete  5.Display  6.Exit\nChoice: ");
        if (scanf("%d", &choice) != 1) break;

        if (choice == 6) break;

        switch (choice) {
            case 1:
                printf("Data: ");
                scanf("%d", &data);
                insertFront(data);
                display();
                break;
            case 2:
                printf("Data: ");
                scanf("%d", &data);
                insertEnd(data);
                display();
                break;
            case 3:
                printf("Insert after (key): ");
                scanf("%d", &key);
                printf("Data: ");
                scanf("%d", &data);
                insertAfter(key, data);
                display();
                break;
            case 4:
                printf("Delete key: ");
                scanf("%d", &key);
                deleteNode(key);
                display();
                break;
            case 5:
                display();
                break;
            default:
                printf("Invalid choice\n");
        }
    }

    freeAll();
    return 0;
}



