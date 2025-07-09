---
title: "[LeetCode - 494.Target Sum] DFS アルゴリズム"
date: 2025-07-09 17:00:00 +09:00
categories: [algorithm]
tags:
  [
    LeetCode,
    Target Sum,
    Algorithm,
    LeetCode,
    DFS,
]
---

## 問題
You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.
- For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".

Return the number of different expressions that you can build, which evaluates to target.

## DFS
開始ノードから子ノードを順番に探索しながら、深さ優先で探索するアルゴリズム。

可能な**すべてのケース（場合の数）**を調べる際に利用されるアルゴリズムです。<br>
（例：経路の数、すべての経路を確認し、条件を満たすものを数える）

主にループ処理または再帰処理を使って実装されます。

## 悩んだ部分
✅ Target Sumを作るロジックはどう作成すれば良いか？

一番重要な質問だと思いました。<br>

順番を変える必要がなかったのは幸いで、混乱せずに済みました。<br>
以前一度解いたことのある問題でしたが、久しぶりに挑戦してみたら内容を思い出せませんでした。<br>

私にとっては DFS（深さ優先探索）の方が BFS（幅優先探索）よりも理解しやすかった記憶があり、DFSを選びました。

🅾️ 全体コード
```kotlin
class Solution {
    fun solution(numbers: IntArray, target: Int): Int {
        var answer = 0

        fun dfs(index: Int, currentSum: Int) {
            if (index == numbers.size) {
                if (currentSum == target) {
                    answer++
                }
                return
            }
            dfs(index + 1, currentSum + numbers[index])
            dfs(index + 1, currentSum - numbers[index])
        }

        dfs(0, 0)
        return answer
    }
}
```

1️⃣ 最初は index = 0、currentSum = 0 から始まる
```kotlin
dfs(0, 0)
```

2️⃣ dfs 関数の中で再び dfs 関数を呼び出す（再帰関数）
```kotlin
fun dfs(index: Int, currentSum: Int) {
    // index == 0 なので、条件は偽になり、そのまま下に進む
    if (index == numbers.size) {
        if (currentSum == target) {
            answer++
        }
        return
    }

    // 再帰関数の呼び出し
    dfs(index + 1, currentSum + numbers[index])
    dfs(index + 1, currentSum - numbers[index])
}
```

3️⃣ ここからは再帰関数をたどりながら計算が始まる。この部分が一番理解しづらく、頭の中で追いかけるのが難しかった。
![DFSの再帰イメージ](/assets/img/post/algorithm/dfs1.png)

indexが加算される理由は？
再帰関数を通じて、index は +1 ずつ加算され、currentSum は +numbers[index] または -numbers[index] として、条件を満たすまで繰り返し実行されます。
```kotlin
dfs(index + 1, currentSum + numbers[index])
dfs(index + 1, currentSum - numbers[index])
```
4️⃣　index が numbers.size と等しくなったら終了。もし targetNumber と一致すれば、answer をインクリメントする。
![DFSの再帰イメージ](/assets/img/post/algorithm/dfs2.png)

## 結論
ノードを実際に描きながら解いてみると、理解が深まった気がします。<br>
currentSumがどのように変化するのかを目で確認できたので、はっきりと理解することができました。<br>
ただし、毎回ノードを描いて解くわけにはいかないので、問題をたくさん解いて慣れていくしかないと思いました。<br>







