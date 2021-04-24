```c
//
//  main.c
//  test01
//
//  Created by jaewoo Kim on 2021/04/21.
//
#include <stdio.h>
#include <stdlib.h>
 
int p_max[10] = {0,0,0,0,0, 0,0,0,0,0} ;//우선순위 비교용 배열
int p_i = 0 ;
int ii = 0 ;

// Link list node
struct Node {
    int data, p;
    struct Node* next;
};

// 자리바꾸기(포인터 바꾸기)
void rotate(struct Node** head_ref, int k)
{
    if (k == 0)
        return;

    struct Node* current = (*head_ref);

    if (current == NULL)
        return;
 
    struct Node* kthNode = current;
 
    while ((current->next) != NULL) {
        current = current->next;
    }
 
    //마지막 노드를 첫번째로
    current->next = (*head_ref);
 
    //head를 head->next로
    (*head_ref) = kthNode->next;

    kthNode->next = NULL;
}

// 입력한 중요도값이 입력한 순서대로가 아닌 입력한 순서 반대순으로 나와서 입력한 순서대로 정렬하기 위한 코드
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
 
// 노드 push
void enqueue(struct Node** head_ref, int new_data, int new_p)
{
    struct Node* new_node
        = (struct Node*)malloc(sizeof(struct Node));
    
    new_node->data = new_data;// 노드의 데이터값을 입력받은 new_data로
    new_node->p = new_p; // 노드의 우선순위를 입력받은 new_p값으로
    new_node->next = (*head_ref);// 다음 노드로 이동
    (*head_ref) = new_node;

    p_max[p_i] = new_node->p ;
    p_i = p_i + 1 ;

}


// 가장 높은 중요도 값 출력/삭제
void dequeque(struct Node** head_ref, int new_data)
{
    struct Node* temp;

    if(*head_ref==NULL)     // 맨처음 노드가 Null일때
        printf("\nQueue is Empty");
    else
    {
        temp = *head_ref; //temp는 맨앞을 가르키는 포인터
        (*head_ref) = (*head_ref)->next; //맨앞을가르키는 포인터를 다음 노드로 설정
        
        printf("\nDeleted data:- %d\tPriority:- %d \n",temp->data, new_data);
        free(temp); // 맨앞의 temp는 메모리 해제(삭제)  따라서 temp노드의 next를 *head_ref가 포인트 하게 된다
    }
}

void printList(struct Node* head)
{
    int i_ref = 10-ii ;
    struct Node* temp = NULL;
    temp = head; //temp를 head로
    while (temp != NULL) { // temp = head 가 Null이 아닐시 반복
        //printf(" %d, %d, %d  ", temp->next, temp->data, temp->p);
        if (temp->p < i_ref) // 맨앞의 우선순위가 i_ref보다 작으면
            printf("%d -> ", temp->p);
        temp = temp->next;
    }
}
 
int main()
{
    int i, j, k, temp_i ;
    int n,a,priority;

    // Start with the empty list
    struct Node* head = NULL;
 
    //데이터와 우선순위값 직접 입력 코드(데이터는 그냥 전부 11로 넣음)
    printf("\n인쇄 요청 수 입력:");
    scanf("%d",&n);
    printf("\nEnter your data and its priorities \n");
    a=0;
    int count = 0;
    while(a<n){
        printf("인쇄 요청의 중요도 입력 %d :", count+=1);
        scanf("%d",&priority);
        enqueue(&head, 11 ,priority);
        a++;
    }
//    enqueue(&head, 11, 3);
//    enqueue(&head, 22, 1);
//    enqueue(&head, 33, 4);
//    enqueue(&head, 44, 5);
//    enqueue(&head, 55, 2);
//    enqueue(&head, 66, 6);
//    enqueue(&head, 77, 7);
//    enqueue(&head, 88, 9);
//    enqueue(&head, 99, 8);


    // 우선순위랑 비교할용으로 쓸 배열 정렬 ( {9,8,7,6,5,4,3,2,1} )
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
    
    // 배열에 정렬한 값 출력용
    for (i = 0; i < 9; ++i)
    {
        printf("%d, ", p_max[i]);
    }
 
    // enqueue한 값 출력
//    printf("\n\nGiven linked list\n");
//    printList(head);

    // enqueue한 값의 순서를 리버스 시켜서 출력
    reverse(&head);//리버스 시키고
    
//    printf("\n\nReversed Linked list \n");
//    printList(head); //리버스 시킨 상태를 출력

    // 우선순위별로 출력
    printf("\n\nReorder Priority \n");
    while (ii < 9)
    {
        printList(head);
        printf("\n");
        rotate(&head, 1);
        
        if(p_max[ii] == head->p){//배열[ii]번째 하고 우선순위값하고 같으면
            printf("data = %d, priority = %d",head->data, head->p);
            dequeque(&head, head->p); //데이터와 우선순위 출력,삭제
            ii += 1;// 배열 index 하나 증가
        }
    }
    printf("\n완료 \n");
}


```
