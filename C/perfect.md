```c


// Iterative C program to reverse a linked list
#include <stdio.h>
#include <stdlib.h>
 
//int p_max = 0, p_max_1 ;
int p_max[10] = {0,0,0,0,0, 0,0,0,0,0} ;
int p_i = 0 ;
int ii = 0 ;


// Link list node 
struct Node {
    int data, p;
    struct Node* next;
};
struct Node* f = NULL;		// front 
//struct Node* r = NULL;

// This function rotates a linked list counter-clockwise and
// updates the head. The function assumes that k is smaller
// than size of linked list. It doesn't modify the list if
// k is greater than or equal to size
void rotate(struct Node** head_ref, int k)
{
    if (k == 0)
        return;
 
    // Let us understand the below code for example k = 4 and
    // list = 10->20->30->40->50->60.
    struct Node* current = (*head_ref);
 
    // current will either point to kth or NULL after this loop.
    // current will point to node 40 in the above example
    int count = 1;
    while (count < k && current != NULL) {
        current = current->next;
        count++;
    }
 
    // If current is NULL, k is greater than or equal to count
    // of nodes in linked list. Don't change the list in this case
    if (current == NULL)
        return;
 
    // current points to kth node. Store it in a variable.
    // kthNode points to node 40 in the above example
    struct Node* kthNode = current;
 
    // list = 10->20->30->40->50->60.
    // current will point to last node after this loop
    // current will point to node 60 in the above example
    while (current->next != NULL) {
        current = current->next;
    }
 
    // Change next of last node to previous head
    // Next of 60 is now changed to node 10
    current->next = (*head_ref);
 
    // Change head to (k+1)th node
    // head is now changed to node 50
    (*head_ref) = kthNode->next;
 
    // change next of kth node to NULL
    // next of 40 is now NULL
    kthNode->next = NULL;
}

// Function to reverse the linked list 
static void reverse(struct Node** head_ref)
{
    struct Node* prev = NULL;
    struct Node* current = *head_ref;
    struct Node* next = NULL;
    while (current != NULL) {
        // Store next
        next = current->next;
 
        // Reverse current node's pointer
        current->next = prev;
 
        // Move pointers one position ahead.
        prev = current;
        current = next;
    }
    *head_ref = prev;
}
 
// Function to push a node 
void enqueue(struct Node** head_ref, int new_data, int new_p)
{
    struct Node* new_node
        = (struct Node*)malloc(sizeof(struct Node));
    new_node->data = new_data;
    new_node->p = new_p;
    new_node->next = (*head_ref);
    (*head_ref) = new_node;

    p_max[p_i] = new_node->p ;
    p_i = p_i + 1 ;

}


// Deletion of highest priority element from the Queue
void dequeue(struct Node** head_ref) //, int new_data, int new_p) 
{
	struct Node* temp = (struct Node*)malloc(sizeof(struct Node));;
    //if (!head_ref)
	if(*head_ref == NULL) 	// Check for Underflow condition
		printf("\nQueue is Empty");
	else
	{
		temp = *head_ref;
		*head_ref = (*head_ref)->next;
		printf("\nDeleted element:- %d\t and Its Priority:- %d\n",temp->data,temp->p);
		free(temp);
	}
}


// Function to print linked list 
void printList(struct Node* head)
{
    int i_ref = 10-ii ;
    struct Node* temp = NULL;
    temp = head;
    while (temp != NULL) {
        //printf(" %d, %d, %d -> ", temp->next, temp->data, temp->p);
        if (temp->p < i_ref) printf("%d -> ", temp->p);
        else break;
        temp = temp->next;
    }
}

/*
void printPriority(struct Node* head)
{
    struct Node* temp = NULL;
    temp = head;
    int jj = 0;

   
    if (p_max[ii] == temp->p)// && (jj == 0))
    {
        printf("\n%d data = %d %d %d\n", temp->next, temp->data, temp->p, ii);
        dequeue(&head);//, temp->data, temp->p);
        ii = ii + 1;
    } 
    
   printf("\n%d data = %d %d %d\n", temp->next, temp->data, temp->p, ii);
}
 */

// Driver code
int main()
{
    int i, j, k, temp_i ;

    // Start with the empty list
    struct Node* head = NULL;
 
    enqueue(&head, 11, 3);
    enqueue(&head, 22, 1);
    enqueue(&head, 33, 4);
    enqueue(&head, 44, 5);
    enqueue(&head, 55, 2);
    enqueue(&head, 66, 6);
    enqueue(&head, 77, 7);
    enqueue(&head, 88, 9);
    enqueue(&head, 99, 8);
    printList(head);
/*
    dequeue(&head); //, 99, 8);
    printList(head);
    dequeue(&head); //, 99, 8);
    printList(head);
*/
    // ordering priority in highest first and lowest last
    // this array values used to print out highest priority first
    for (j = 0; j < 9; ++j)
    { 
        for (k = j+1 ; k < 9-ii; ++k)
        { 
            if (p_max[j] < p_max[k])
            {
                temp_i = p_max[j] ;
                p_max[j] = p_max[k];
                p_max[k] = temp_i;
            }
        }
    }

    // check whether ordered in 
    printf("\npriority linted in highest to lowest\n");
    for (i = 0; i < 9; ++i)
    {
        printf("%d, ", p_max[i]);
    }
 
    printf("\n\nGiven linked list\n");
    printList(head);
    
    reverse(&head);
    printf("\n\nReversed Linked list \n");
    printList(head);

    printf("\n\n############# Reorder Priority #############\n"); 
    while (ii < 9)
    {      
        
        if (p_max[ii] == head->p)// && (jj == 0))
        {
            printf("\n%d data = %d %d %d\n", head->next, head->data, head->p, ii);
            dequeue(&head);//, temp->data, temp->p);
            ii = ii + 1;
        } 
        printList(head);
        printf("\n");

        rotate(&head, 1);
    }
   
    printf("\n good lock!!! \n");
}

```
