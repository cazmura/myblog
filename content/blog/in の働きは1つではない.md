+++
title = "in の働きは1つではない"
date = "2026-01-01T16:46:44+09:00"
draft = false
tags = ["python"]
images = []
+++

in 演算子というものがある。

これは、どのような演算式の中で使われるかで、働きが変わる。

for 文で使われている場合「要素を1つずつ取り出して変数に入れる」という働きになる。

それ以外の場合「要素が中にあるか？（True/False）」という働きになる。

### サンプルコード

```
target_text = 'hello'
count_dict = {}
for char in target_text:
    if char in count_dict:
        count_dict[char] += 1
    else:
        count_dict[char] = 1
print(count_dict)
```

