# 前K个高频元素

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
你可以按任意顺序返回答案。

## 优先队列
感觉优先队列其实和C++解法里面的小根堆差不多，都是每次让最小的元素出队，维护队列长度为k，这样就可以保证最终的队列就是我们所要的数组。
用了HashMap，效率还是比较差，题目没有给出数字的范围，如果给出了应该是可以用数组优化的。
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        if (nums==null)
        {
            return nums;
        }
        Map<Integer,Integer> map=new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        //维护优先队列长度始终为k
        PriorityQueue<Integer> pq=new PriorityQueue<>(Comparator.comparingInt(map::get));
        for(int num: map.keySet()){
            pq.add(num);
            if(pq.size() > k) {
                pq.poll();
            }
        }
        int[] res=new int[k];
        for (int i = 0; i < k; i++) {
            res[k-i-1]=pq.poll();
        }
        return res;
    }
}
```

## 使用LFU解法
非常巧妙，来自评论区https://leetcode-cn.com/problems/top-k-frequent-elements/solution/fei-biao-zhun-jie-fa-shou-gong-chuang-jian-lfutong/

对输入数组进行扫描, 添加至 LFU, 添加操作每步均为 O(1) 整体复杂度为 O(N)。

**LFU（Least Frequently Used ，最近最少使用算法）**也是一种常见的缓存算法。

顾名思义，LFU算法的思想是：**如果一个数据在最近一段时间很少被访问到，那么可以认为在将来它被访问的可能性也很小。因此，当空间满时，最小频率访问的数据最先被淘汰。**

LFU 算法的描述：

设计一种缓存结构，该结构在构造时确定大小，假设大小为 K，并有两个功能：

set(key,value)：将记录(key,value)插入该结构。当缓存满时，将访问频率最低的数据置换掉。

get(key)：返回key对应的value值。
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        LFU lfu = new LFU();
        for (int val : nums) {
            lfu.put(val);
        }
        return lfu.getTopKFreqent(k);
    }

    private class LFU {
        HashMap<Integer, Node> cache;
        DoublyLinkedList head;
        DoublyLinkedList tail;

        public LFU() {
            cache = new HashMap<>();
            head = new DoublyLinkedList(0);
            tail = new DoublyLinkedList(0);
            head.next = tail;
            tail.prev = head;
        }

        public void put(int key) {
            Node node = cache.get(key);
            if (node == null) {
                node = new Node(key);
                DoublyLinkedList prevList = tail.prev;
                if (prevList.freq == node.freq) {
                    prevList.addToTail(node);
                } else {
                    DoublyLinkedList newList = new DoublyLinkedList(node.freq);
                    newList.addToTail(node);
                    addList(newList, prevList);
                }
            } else {
                node.freq++;
                DoublyLinkedList curList = node.curList;
                DoublyLinkedList prevList = curList.prev;
                curList.remove(node);
                if (curList.head.next == curList.tail) {
                    removeList(curList);
                }
                if (prevList.freq == node.freq) {
                    prevList.addToTail(node);
                } else {
                    DoublyLinkedList newList = new DoublyLinkedList(node.freq);
                    newList.addToTail(node);
                    addList(newList, prevList);
                }
            }
            cache.put(key, node);
        }

        public int[] getTopKFreqent(int k) {
            int[] res = new int[k];
            int cur = 0;
            Node curNode = head.next.head.next;
            while (cur < k) {
                // System.out.println(cur + ", " + curNode.key + ", " + curNode.freq + ", "+ curNode.next.key + ", " + curNode.curList.freq);
                res[cur++] = curNode.key;
                curNode = curNode.next;
                if (curNode == curNode.curList.tail) {
                    curNode = curNode.curList.next.head.next;
                }
            }
            return res;
        }

        private void addList(DoublyLinkedList newList, DoublyLinkedList prevList) {
            prevList.next.prev = newList;
            newList.next = prevList.next;
            prevList.next = newList;
            newList.prev = prevList;
        }

        private void removeList(DoublyLinkedList curList) {
            curList.next.prev = curList.prev;
            curList.prev.next = curList.next;
            curList.next = null;
            curList.prev = null;
        }
    }

    private class DoublyLinkedList {
        DoublyLinkedList prev;
        DoublyLinkedList next;
        Node head;
        Node tail;
        int freq;

        public DoublyLinkedList(int freq) {
            this.freq = freq;
            head = new Node(0);
            tail = new Node(0);
            head.next = tail;
            tail.prev = head;
            // from head/tail can get curList
            head.curList = this;
            tail.curList = this;
        }

        public void addToTail(Node node) {
            tail.prev.next = node;
            node.prev = tail.prev;
            node.next = tail;
            tail.prev = node;
            node.curList = this;
        }

        public void remove(Node node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
            node.next = null;
            node.prev = null;
            node.curList = null;
        }
    }

    private class Node {
        int key;
        int freq;
        Node prev;
        Node next;
        DoublyLinkedList curList;

        public Node(int key) {
            this.key = key;
            this.freq = 1;
        }
    }
}
```
效率非常高，学到了。
