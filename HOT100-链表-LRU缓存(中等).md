### HOT100-链表-146.LRU缓存(简单)

#### 题目描述

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

 

**示例：**

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

 

**提示：**

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`
- 最多调用 `2 * 105` 次 `get` 和 `put`



#### 提交记录

##### 1. 字典+列表求解(首次提交)

为了逐出最久未使用的关键字，不仅用了dict，还用了list来操作。每次调用key，就找到list中的key，并remove后重新append。很明显不满足时间复杂度的要求。

```python
class LRUCache:
    def __init__(self, capacity: int):
        self.capacity=capacity#2
        self.dicList=[]
        self.diction={}#2:1

    def get(self, key: int) -> int:
        value=self.diction.get(key,-1)
        if value!=-1:
            self.dicList.remove(key)
            self.dicList.append(key)
        return self.diction.get(key,-1)

    def put(self, key: int, value: int) -> None:
        if key in self.dicList:
            self.dicList.remove(key)
        self.dicList.append(key)
        self.diction[key]=value
        if len(self.dicList)>self.capacity:
            del self.diction[self.dicList.pop(0)]
        return None
```

<img src="C:\Users\Gaga\AppData\Roaming\Typora\typora-user-images\image-20240301151156276.png" alt="image-20240301151156276" style="zoom:50%;" />



##### 2. 字典+双向链表求解

````java
class LRUCache {
    int cap;
    LinkedHashMap<Integer, Integer> cache = new LinkedHashMap<>();
    public LRUCache(int capacity) { 
        this.cap = capacity;
    }

    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        // 将 key 变为最近使用
        makeRecently(key);
        return cache.get(key);
    }

    public void put(int key, int val) {
        if (cache.containsKey(key)) {
            cache.put(key, val);
            makeRecently(key);
            return;
        }

        if (cache.size() >= this.cap) {
            int oldestKey = cache.keySet().iterator().next();
            cache.remove(oldestKey);
        }

        cache.put(key, val);
    }

    private void makeRecently(int key) {
        int val = cache.get(key);
        cache.remove(key);
        cache.put(key, val);
    }
}
````

也有更好的方法，但是过于复杂...



#### 代码总结

- 双向链表的操作

  ````java
  //创建双向链表
  LinkedHashMap<Integer, Integer> cache = new LinkedHashMap<>();
  
  //获取key的集合
  //迭代器获取第一个值
  int oldestKey = cache.keySet().iterator().next();
    
  //判断迭代器是否有next值
  while (iterator.hasNext()) 
  
  //移除键值对
  cache.remove(oldestKey)
  ````
  

