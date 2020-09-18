# 2020q3 Homework1 (quiz1)

contributed by < `Ylowy` >

## Outline
1. 重新回答[第 1 周測驗題](https://hackmd.io/@sysprog/sysprog2020-quiz1)，連同附帶的「延伸問題」。
    解釋程式運作原理時，應提供對應的 Graphviz 圖例，可參照 Linked List 題目 1 + 分析
    
2. 比照 課前測驗參考解答: 
    Q1 和 Linked list 題目分析 的模式來撰寫共筆，需要詳細分析自己的思路、參閱的材料 (以第一手材料為主，包含 C 語言規格書的章節)，以及進行相關實驗。
    
## 1. 重新回答第一周測驗題

型態先定義長這樣
```c=
typedef struct __node {                   
    int value;
    struct __node *next;
} node_t;
```
```c=
void add_entry(node_t **head, int new_value)
{
    node_t **indirect = head;

    node_t *new_node = malloc(sizeof(node_t));
    new_node->value = new_value;
    new_node->next = NULL;

    assert(new_node);
    
    while (*indirect)
        indirect = &(*indirect)->next;
    *indirect = new_node;
}
```
```c=
node_t *find_entry(node_t *head, int value)
{
    node_t *current = head;
    for (; current && current->value != value; current = current->next)
        /* interate */;
    return current;
}
```
```c=
void remove_entry(node_t **head, node_t *entry)
{
    node_t **indirect = head;

    while ((*indirect) != entry)
        indirect = &(*indirect)->next;

    *indirect = entry->next;
    free(entry);
}
```
這邊卡真久，關鍵在for迴圈中
```c=
void swap_pair(node_t **head) {
  for (node_t **node = head; *node && (*node)->next;node = &(*node)->next->next){
    node_t *tmp = *node;
    *node = (*node)->next;
    tmp->next = (*node)->next;
    (*node)->next = tmp;
  }
}

```
[Ylowy/Lab0-c](https://hackmd.io/1BvGp-2oQuqYUzD1NtZU6g?view) 參考我之前寫的 reverse queue 的方法，省一個空間
```c=
void reverse(node_t **head)
{
    node_t * tail = *head;
	while(tail->next)
    	tail = tail->next;
    tail->next = *head;
    node_t *ptr = (*head)->next->next;
    
    while ((*head)->next != tail) {
        (*head)->next->next = tail->next;
        tail->next = (*head)->next;
        (*head)->next = ptr;
        ptr = ptr->next;
    }
    tail = *head;
    *head = (*head)->next;
    tail->next = NULL;
}
```


## 2. 比照課前測驗參考解答