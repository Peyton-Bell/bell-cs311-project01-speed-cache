1. Phase 1: The linked list and map entries are connected through the node itself. For the map the setup is map[key] = node where node is a reference to the actual node in our linked list. The linked list is just a bunch of those node values in a list. This is how we can use the map for lookup and make it O(1) instead of O(n) like you normally have with lists. The reason we store the node pointer instead of the actual value is so we can call things like node.next or node.prev to change the pointers within the node so we can access and modify the list in O(1) time.

Example:

Map{"a": node} --> Node a in linked
Map{"b": node} --> Node b in linked list

linked list: head <-> a <-> b <-> tail

## Complexity

- Get: dict lookup O(1) time complextiy, and then move_to_front which rewrites the pointers of a node moving it to the beginning of the linked list. This is O(1) average as well
- Put: dict lookup O(1) average and then we either update the node and use move_to_front (still O(1) complexity) or we insert a new node if the key is new. If our list capacity is full we evict the last node in our list and then insert the new one and use add_to_front on it. Every step is O(1) in this case so overall it is also an O(1) time complexity
- Eviction: Our least recently used (lru) node is always at tail.prev so we don't need to do a lookup here. We use lru.key to find the node's key and then call remove on the node and del map[key] for the dict. This process is also O(1) complexity.

## hitrate results

Simulating page requests across cache capacities...

capacity= 5 hit rate=16.16%
capacity= 10 hit rate=31.57%
capacity= 20 hit rate=56.77%
capacity= 50 hit rate=81.08%
capacity= 100 hit rate=83.37%

Hit rate increased as the capacity increased because the larger the cache is the less it has to evict out nodes. This means that future requests are more likely to already be cached and found instantly. There seems to be some diminishing returns past around 50 though. From 50-100 capacity the hit rate only went up 2%, likely due to 50 already holding the most of the web pages that are being requested.
