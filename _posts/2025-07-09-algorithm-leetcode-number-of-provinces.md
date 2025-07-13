---
title: "[LeetCode - 547. Number of Provinces] 訪問済みを記録するアルゴリズム"
date: 2025-07-13 16:00:00 +09:00
categories: [Algorithm]
tags:
  [
    LeetCode,
    Number of Provinces,
    Algorithm,
    LeetCode,
    訪問チェック,
]
---

## 問題
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.
You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

## 問題解決過程
🅾️ 全体コード
```kotlin
class Solution {
    fun findCircleNum(isConnected: Array<IntArray>): Int {
        var answer = 0
        val n = isConnected.size
    
        val visited = BooleanArray(n)

        for (start in 0 until n) {
            if (visited[start].not()) {
                answer += 1
                dfs(n, start, visited, isConnected)
            }
        }
        return answer
    }

    fun dfs(n: Int, start: Int, visited: BooleanArray, isConnected: Array<IntArray>) {
        visited[start] = true

        for (end in 0 until n) {
            if (isConnected[start][end] == 1 && visited[end].not()) {
                dfs(n, end, visited, isConnected)
            }
        }
    }
}
```

1️⃣ 各コンピュータは、必ず自分自身のネットワークには接続されているため、まずは訪問状態を記録する BooleanArrayを作りました。<br>
そして、まだ訪問していないノード（コンピュータ）に出会った場合は、新しいネットワークと見なし、answerを1増やします。<br>
その後、dfsを呼び出して、そのネットワークに属するすべてのノードを探索します。<br>
```kotlin
 val visited = BooleanArray(n)

        for (start in 0 until n) {
            if (visited[start].not()) {
                answer += 1
                dfs(n, start, visited, isConnected)
            }
        }
```

2️⃣ このdfsでは、現在のコンピュータ（start）から他のノード（end）への接続を確認します。<br>
具体的には、computers[start][end] == 1（接続されている）かつvisited[end]==false（未訪問）であれば、<br>
そのノードを訪問し、再帰的にdfsを呼び出してネットワーク全体を探索していきます。<br>
dfs(n, end, visited, computers) でendを渡している理由は、次に訪問すべきノードとしてendを指定しているからです。<br>
```kotlin
fun dfs(n: Int, start: Int, visited: BooleanArray, computers: Array<IntArray>) {
    visited[start] = true

    for (end in 0 until n) {
        if (computers[start][end] == 1 && visited[end].not()) {
            dfs(n, end, visited, computers)
        }
    }
}
```
## 結論
各コンピュータの訪問状況を確認することで、不要な探索を防いだほうが良い。<br>
結構典型的なDFS問題だけど、やはり慣れていないと訪問チェックとか思い出すことは難しいなと思います。
