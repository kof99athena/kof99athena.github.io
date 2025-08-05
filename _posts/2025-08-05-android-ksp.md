---
title: "[Android] KSPとは？KSP vs KAPT"
date: 2025-08-05 08:00:00 +09:00
categories: [Android]
tags:
  [
    Android,
    KSP,
    KAPT,
]
---

## KSPとは？
アノテーションが付けられたコードをコンパイル時に解析し、必要なコードを自動生成するツールです。<br>
たとえば、@Entityアノテーションが付けられたクラスを読み取り、対応するファイルを自動的に生成します。<br>
エンジニアが手動で書く必要のある繰り返しのコードを省略できるため、開発効率が大幅に向上します。<br>

```kotlin
@Entity
data class User(val id: Int, val name: String)
```
```kotlin
// 自動生成される例
class User_Impl : RoomEntity {
    // フィールドのマッピングやクエリサポートなど
}
```

## KSPはどこで使われているのか？
- Room: @Entity, @Dao, @Database<br>
- Moshi: @JsonClass(generateAdapter = true)<br>
- AutoService: @AutoService によるSPI登録<br>
- Anvil (Square): Hilt/Daggerに関連するDIのボイラープレート削減<br>

## KAPTとKSPの違い
| 項目           | KAPT                                | KSP                                   |
|----------------|-------------------------------------|----------------------------------------|
| ベース         | Java Annotation Processor           | Kotlin専用のシンボルプロセッサ           |
| ビルド速度     | 遅い（全ソースを再処理）            | 速い（増分ビルドをサポート）             |
| Kotlin対応      | 低い                                 | 高い                                    |
| サポート状況   | 将来的に縮小される可能性がある      | JetBrainsが公式サポート中               |

## ただし…HiltではまだKAPTが必要
| アノテーション                            | 処理方法   | 説明                                                   |
|-----------------------------------------|------------|--------------------------------------------------------|
| `@HiltViewModel`                        | ❌ KSP不可  | ViewModelのFactory生成・依存性注入用                    |
| `@Inject`, `@Module`, `@Provides`       | ❌ KSP不可  | Dagger/Hiltの依存性注入関連（KAPT必須）                |
| `@JsonClass(generateAdapter = true)`    | ✅ KSP対応  | MoshiではKSPを完全サポート                             |
| `@Entity`, `@Dao`, `@Database`          | ✅ KSP推奨  | Roomの最新バージョンではKSPの使用がおすすめされる          |

## Jetpack Composeのアノテーションは？
以下のような@Composable, @Previewなどのアノテーションは<b>KSPで処理されません。</b>
```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```
これらは Jetpack Compose用の専用コンパイラプラグインによって処理されます。<br>
つまり、「アノテーション=KSPが処理する」というわけではありません。


