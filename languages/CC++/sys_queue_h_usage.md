# Using `sys/queue.h` for lists, stacks, and queues.

## Minimal tail Queue example

```c
#include <stdio.h>
#include <sys/queue.h>


typedef struct node {
    // declare the linked member variable
    TAILQ_ENTRY(node) tailq;
    int key;
    int value;
}node;

// declare the type of the tail queue as q_type
TAILQ_HEAD(q_type, node);

int main() {

    struct q_type queue;        // declare a queue variable
    TAILQ_INIT(&queue);         // init the queue
    node * n = malloc(sizeof(struct node));
    n->value = 1;
    n->key = 1;
    TAILQ_INSERT_TAIL(&queue, n, tailq);    // insert at tail
    node* entry;
    TAILQ_FOREACH(entry, &queue, tailq)     // iterate the queue elements, note that the first parameter is the iterator
        printf("%d->%d\n", entry->key, entry->value);
    return 0;
}
```

## single linked list example (`SLIST`)

```c
#include <sys/queue.h>

struct node {
    // declare the linked member variable
    SLIST_ENTRY(node) next;
    int v;
};

// declare the list type as list_type
SLIST_HEAD(list_type, node);



bool dfs_find_path(struct list_type* adj, int* visit, int n, int start, int target) {
    if (start == target)
        return true;
    
    struct node* entry;
    SLIST_FOREACH(entry, &adj[start], next) {
        int v = entry->v;
        if (visit[v]) continue;
        visit[v] = 1;
        bool ret = dfs_find_path(adj, visit, n, v, target);
        if (ret)
            return true;
    }
    return false;
}


bool findWhetherExistsPath(int n, int** graph, int graphSize, int* graphColSize, int start, int target){
    
    struct list_type* adj = malloc(sizeof(struct list_type) * n);
    int visit[n];
    
    for (int i = 0; i < n; i++)
        SLIST_INIT(adj+i);      // init lists
    memset(visit, 0, sizeof(visit));

    for (int i = 0; i < graphSize; i++) {
        struct node* entry = malloc(sizeof(struct node));
        entry->v = graph[i][1];
        SLIST_INSERT_HEAD(&adj[graph[i][0]], entry, next);      // insert elements at the head (O(1)), if at tail will be O(n) time complexity.
    }
        

    bool ret = dfs_find_path(adj, visit, n, start, target);

    for (int i = 0; i < n; i++) {
        while (!SLIST_EMPTY(adj+i)) {       // delete all the elements in the list
            struct node* entry = SLIST_FIRST(adj+i);
            SLIST_REMOVE_HEAD(adj+i, next);
            free(entry);
        }
    }
    free(adj);

    return ret;
}
```
