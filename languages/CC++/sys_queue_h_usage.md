# Using `sys/queue.h` for lists, stacks, and queues.

## Minimal tail Queue example

```c
#include <stdio.h>
#include <sys/queue.h>


typedef struct node {
    TAILQ_ENTRY(node) tailq;
    int key;
    int value;
}node;

TAILQ_HEAD(q_type, node);

int main() {

    struct q_type queue;
    TAILQ_INIT(&queue);
    node * n = malloc(sizeof(struct node));
    n->value = 1;
    n->key = 1;
    TAILQ_INSERT_TAIL(&queue, n, tailq);
    node* entry;
    TAILQ_FOREACH(entry, &queue, tailq)
        printf("%d->%d\n", entry->key, entry->value);
    return 0;
}
```
