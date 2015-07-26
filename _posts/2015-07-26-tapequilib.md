---
layout: post
title: Codility - TapeEquilibrium in Ruby
---

[Intro to this series](http://blog.natecollings.com/posts/codility/)

[I recommend attempting the lessons first before you read my solutions.](https://codility.com/programmers/lessons/)

The description of this problem is rather long, so I will try to summarize it here (you can read the full description in the link above):

* You are given an array `A` of integers of length `N`
* An array can be "split" at index `P`
* The "difference" between the two splits is the absolute difference of the sum of each
* You are tasked with finding the minimum difference between all possible splits

The value restrictions were as follows:

* `N` is in the range `[2..100,000]`
* Each element of `N` is an integer within the range `[-1000, 1000]`

When I first started doing code challenges I would typically ignore the the input/value restriction sections (particularly when using a dynamically typed language where I didn't need to worry as much about whether to use int/floats/doubles). However, these typically offer very important tips about the type of solution you need to find. In this case, `N` could be of length `100,000` -- given the speed requirements to solve the problem, it seems likely that you would need a solution that runs in `O(n)` or better. ([Codility has a good overview of time complexity](https://codility.com/media/train/1-TimeComplexity.pdf) that I recommend reading, with some good rules of thumb.)

A naive algorithm might be:

- Iterate through each element in `N`, keeping track of the current sum
- For each element, iterate through the rest of the array to find the right pivot value
- Find the difference, and store it as min if necessary

This would be in `O(n^2)`, which is much too slow for the input sizes. (Ick, nested for loops.)

The better solution is to:

- Iterate through the array once to get the total sum
- Init the left pivot to be `A[0]` and the right pivot to be `sum - A[0]`
- Init min to be the absolute difference of the right and left pivots
- Iterate through the array, and add the current value to the left and subtract it from the right
- Find the absolute differences again, and store as min if it is less than the current min

My solution in Ruby looked like this:

```ruby
def solution(a)
  left_side = a[0]
  right_side = a[1..-1].inject { |sum, x| sum + x }
  min = (right_side - left_side).abs
  a[1..-1].each do |val|
    diff = (right_side - left_side).abs
    min = diff if diff < min
    left_side += val
    right_side -= val
  end
  min
end
```

Technically this solution runs in `2N`, but we drop the constants in Big-Oh notation.

As is typically the case, I had the right idea initially but failed some of the test cases. After several tries I was able to use the solution as shown above [to pass all of the test cases](https://codility.com/demo/results/demoKBJHSW-XJJ/).
