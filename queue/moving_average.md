# Moving Average from Data Stream

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

Implement the `MovingAverage` class:

`MovingAverage(int size)` Initializes the object with the size of the window `size`.

`double next(int val)` Returns the moving average of the last `size` values of the stream.

Example:

```
Input
["MovingAverage", "next", "next", "next", "next"]
[[3], [1], [10], [3], [5]]
Output
[null, 1.0, 5.5, 4.66667, 6.0]

Explanation
MovingAverage movingAverage = new MovingAverage(3);
movingAverage.next(1); // return 1.0 = 1 / 1
movingAverage.next(10); // return 5.5 = (1 + 10) / 2
movingAverage.next(3); // return 4.66667 = (1 + 10 + 3) / 3
movingAverage.next(5); // return 6.0 = (10 + 3 + 5) / 3
```

```go
type MovingAverage struct {
    Size int
    Values []int
}


func Constructor(size int) MovingAverage {
    return MovingAverage{Size: size, Values: []int{}}
}


func (this *MovingAverage) Next(val int) float64 {
    if len(this.Values) == this.Size {
        this.Values = this.Values[1:]
    }
    
    this.Values = append(this.Values, val)
    
    sum := 0
    
    for _, num := range this.Values {
        sum += num
    }
    
    return float64(sum) / float64(len(this.Values))
}


/**
 * Your MovingAverage object will be instantiated and called as such:
 * obj := Constructor(size);
 * param_1 := obj.Next(val);
 */
```