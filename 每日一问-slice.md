# 每日一问

slice 和 map, channel, 指针等有时一起被归为属于带有"引用"属性的类型,  但其中slice又和其他其他几个不太一样，有时会展现出非"引用"类型的属性，例如:

```go
//append 后要赋值
s := make([]int, 0)j
s = append(s, 10)

//如果需要在函数体内修改slice且对外可见，必须传指针
func extend(s *[]int, n int) error
//或者和append一样，把slice重新返回
func extend(s []int, n int) (result []int, err error)
```

 这是因为slice值本身可以看做以下这样一个结构体:

```go
type slice struct {
  ElemType *data
  int len
  int capacity
}
```

因而从这个角度来看slice更像是一个"值类型"，通过其内的一个指针产生"引用"的效果.

那么问题来了, 为什么map等不需要类似这样的操作，为什么我们几乎很少用到指向map的指针? 比如

```go
m := make(map[string]int)
...
m = delete(m, "well")  //resembles slice append, doesn't compile
func (m *map[string]int)  //compiles perfectly but rarely used 
```

go为什么不将slice和其他"引用"类型统一用法 ?

