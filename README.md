
# GoのSliceをSortする(sort.Sliceとsort.SliceStable)

本記事のコード全体は以下。
https://play.golang.org/p/Lnj8kAwWxyy

## 安定ソート(stable sort)とは
まずsortは[安定ソート](https://ja.wikipedia.org/wiki/%E5%AE%89%E5%AE%9A%E3%82%BD%E3%83%BC%E3%83%88)かどうかで挙動が異なる

安定ソート(stable sort)については以下がわかりやすいので引用させていただいた。
> 安定ソート（あんていソート、stable sort）とは、ソート（並び替え）のアルゴリズムのうち、同等なデータのソート前の順序が、ソート後も保存されるものをいう。つまり、ソート途中の各状態において、常に順位の位置関係を保っていることをいう。
> たとえば、学生番号順に整列済みの学生データを、テストの点数順で安定ソートを用いて並べ替えたとき、ソート後のデータにおいて、同じ点数の学生は学生番号順で並ぶようになっている。

引用元 : [安定ソート - Wikipedia](https://ja.wikipedia.org/wiki/%E5%AE%89%E5%AE%9A%E3%82%BD%E3%83%BC%E3%83%88)

## Goのsort packageのSliceとSliceStable
Goのsort packageには、安定ソートを保証しないSlice関数と安定ソートを保証するSliceStableが存在する。

それぞれ以下で実装とその結果を記す
### sort.Slice

```go
func Slice(slice interface{}, less func(i, j int) bool)
```
安定しているという保証なしのソートを行う。
第二引数で与えたless関数を使用して、第一引数で与えたSliceをソートする。
安定ソートを行うには、SliceStableを使用する。


#### 実装
```go
		people := []Person{
    		{Name: "V", Age: 3},
    		{Name: "K", Age: 3},
    		{Name: "Y", Age: 3},
    		{Name: "A", Age: 4},
    		{Name: "E", Age: 3},
    		{Name: "D", Age: 1},
    		{Name: "C", Age: 3},
    		{Name: "X", Age: 2},
    		{Name: "B", Age: 3},
    	}
    
    	sort.Slice(people, func(i, j int) bool { return people[i].Name < people[j].Name })
    	fmt.Printf("NameでSort(Not-Stable):%+v\n", people)
    
    	sort.Slice(people, func(i, j int) bool { return people[i].Age < people[j].Age })
    	fmt.Printf("AgeでSort(Not-Stable):%+v\n", people)
```

#### 実行結果

```

NameでSort(Not-Stable):[{Name:A Age:4} {Name:B Age:3} {Name:C Age:3} {Name:D Age:1} {Name:E Age:3} {Name:K Age:3} {Name:V Age:3} {Name:X Age:2} {Name:Y Age:3}]
AgeでSort(Not-Stable):[{Name:D Age:1} {Name:X Age:2} {Name:V Age:3} {Name:C Age:3} {Name:E Age:3} {Name:K Age:3} {Name:B Age:3} {Name:Y Age:3} {Name:A Age:4}]
```

### sort.SliceStable

```go
func SliceStable(slice interface{}, less func(i, j int) bool)
```

安定ソートを行う

```go
people2 := []Person{
		{Name: "V", Age: 3},
		{Name: "K", Age: 3},
		{Name: "Y", Age: 3},
		{Name: "A", Age: 4},
		{Name: "E", Age: 3},
		{Name: "D", Age: 1},
		{Name: "C", Age: 3},
		{Name: "X", Age: 2},
		{Name: "B", Age: 3},
	}

	sort.SliceStable(people2, func(i, j int) bool { return people2[i].Name < people2[j].Name })
	fmt.Printf("NameでSort(Stable):%+v\n", people2)

	sort.SliceStable(people2, func(i, j int) bool { return people2[i].Age < people2[j].Age })
	fmt.Printf("AgeでSort:%+v\n", people2)
}
```

#### 実行結果

```
NameでSort(Stable):[{Name:A Age:4} {Name:B Age:3} {Name:C Age:3} {Name:D Age:1} {Name:E Age:3} {Name:K Age:3} {Name:V Age:3} {Name:X Age:2} {Name:Y Age:3}]
AgeでSort:[{Name:D Age:1} {Name:X Age:2} {Name:B Age:3} {Name:C Age:3} {Name:E Age:3} {Name:K Age:3} {Name:V Age:3} {Name:Y Age:3} {Name:A Age:4}]
```

## 参考にさせていただいた記事
[sort - The Go Programming Language](https://golang.org/pkg/sort/#SliceStable)
[安定ソート - Wikipedia](https://ja.wikipedia.org/wiki/%E5%AE%89%E5%AE%9A%E3%82%BD%E3%83%BC%E3%83%88)

