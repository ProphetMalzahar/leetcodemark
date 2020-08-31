# 钥匙和房间

先看题目：

有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N-1，并且房间里可能有一些钥匙能使你进入下一个房间。

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j] 由 [0,1，...，N-1] 中的一个整数表示，其中 N = rooms.length。 钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。

最初，除 0 号房间外的其余所有房间都被锁住。

你可以自由地在房间之间来回走动。

如果能进入每个房间返回 true，否则返回 false。

示例 1：

输入: [[1],[2],[3],[]]

输出: true

解释:  

我们从 0 号房间开始，拿到钥匙 1。之后我们去 1 号房间，拿到钥匙 2。然后我们去 2 号房间，拿到钥匙 3。最后我们去了 3 号房间。

由于我们能够进入每个房间，我们返回 true。

示例 2：

输入：[[1,3],[3,0,1],[2],[0]]

输出：false

解释：我们不能进入 2 号房间。

提示：

1 <= rooms.length <= 1000

0 <= rooms[i].length <= 1000

所有房间中的钥匙数量总计不超过 3000。


来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/keys-and-rooms

**这题有深度优先和广度优先两种做法**

**思路其实差不多，都是用boolean数组保存已访问的节点。**

## 深度优先搜索

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        boolean[] key =new boolean[rooms.size()];
        dfs(rooms,key,0);
        //如果有一个房间无法到达就返回false
        for (boolean b : key) {
            if (!b) {
                return false;
            }
        }
        return true;
    }
    public void dfs(List<List<Integer>> rooms,boolean[] key,int index)
    {
        //已到过
        if (key[index])
        {
            return;
        }
        key[index]=true;
        List<Integer> room=rooms.get(index);
        //根据在此房间中拿到的钥匙去其它房间
        for (Integer integer : room) {
            dfs(rooms, key, integer);
        }
    }
}
```

## 广度优先搜索

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size(), num = 0;
        boolean[] vis = new boolean[n];
        Queue<Integer> que = new LinkedList<Integer>();
        vis[0] = true;
        que.offer(0);
        while (!que.isEmpty()) {
            int x = que.poll();
            num++;
            for (int it : rooms.get(x)) {
                if (!vis[it]) {
                    vis[it] = true;
                    que.offer(it);
                }
            }
        }
        return num == n;
    }
}
```

