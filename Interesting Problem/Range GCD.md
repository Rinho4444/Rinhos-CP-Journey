Source: https://www.acmicpc.net/problem/12858
# Problem Summary: GCD Query and Range Addition

You are given a sequence of N natural numbers (1 ≤ N ≤ 100,000) and Q operations (1 ≤ Q ≤ 100,000). The sequence elements are natural numbers between 1 and 1 billion. There are two types of operations:

1. **Add Operation (T, A, B):** Add the value T to all elements from index A to index B (inclusive).
2. **GCD Query (T = 0, A, B):** Find and print the greatest common divisor (GCD) of the elements from index A to index B (inclusive).

## Key Concepts and Challenges

At first glance, this problem seems like a typical Segment Tree Lazy Update problem. However, a closer look reveals a challenge: simply adding a value to a range of numbers doesn't maintain the relationship needed to compute the GCD efficiently. This is because the GCD of updated numbers can't be derived directly from the original sequence using standard segment tree operations.

### Important GCD Properties

1. **GCD Associativity:** $$\text{GCD}(a, b, c) = \text{GCD}(a, \text{GCD}(b, c))$$
2. **Reduction Property:** If $$a < b$$, then $$\text{GCD}(a, b) = \text{GCD}(a, b - a)$$

Using these properties, we can derive that:

$$ \text{GCD}(a, b, c) = \text{GCD}(a, b - a, c - b) $$

In general:

$$ \text{GCD}(x_1, x_2, x_3, ..., x_{n-1}, x_n) = \text{GCD}(x_1, x_2 - x_1, x_3 - x_2, ..., x_{n-1} - x_{n-2}, x_{n} - x_{n-1}) $$

This insight helps us transform the problem into one where we can handle range updates more effectively.

## Solution Approach

Given the sequence $$x_1, x_2, \dots, x_n$$, we create a new sequence $$y_n$$ where:

- $$y_1 = x_1$$
- $$y_2 = x_2 - x_1$$
- $$y_3 = x_3 - x_2$$
- $$\dots$$
- $$y_n = x_n - x_{n-1}$$

This new sequence helps us perform efficient operations on ranges and compute the GCD. 

### First Type of Operation (Add Operation)

When adding a value T to a range, instead of adding T to each element $$x_i$$ directly, we modify the corresponding entries in the sequence $$y$$:

- Update the $$y_l$$ element by adding T.
- Update the $$y_{r+1}$$ element by subtracting T.

This modification allows us to perform range updates efficiently without directly affecting the GCD of the sequence.

### Second Type of Operation (GCD Query)

For a query asking for the GCD of a range from index A to B, we compute:

$$ \text{GCD}(\text{SUM}(y_1, y_2, \dots, y_l), \text{GCD}(y_{l+1}, y_{l+2}, \dots, y_r)) $$

Where:

- $$\text{SUM}(y_1, y_2, \dots, y_l)$$ gives the cumulative sum of the first $$l$$ elements of sequence $$y$$, which corresponds to the value $$x_l$$ in the original sequence.
- The GCD of the remaining elements $$y_{l+1}, y_{l+2}, \dots, y_r$$ gives the GCD of the range in the original sequence.

You can try to prove it yourself for practicing :>

## Final Solution Outline

We first build the sequence $$y$$ based on the initial sequence $$x$$. Then, for each operation:

- **For an Add Operation (T, A, B):** Update the segment tree to reflect the changes in sequence $$y$$.
- **For a GCD Query (T = 0, A, B):** Use the segment tree to compute the GCD of the range efficiently.

## Code Implementation

```cpp
// Initialize sequence y
y[1] = x[1];
for(int i = 2; i <= n; i++) {
    y[i] = x[i] - x[i - 1];
}

// Solve the operations
int num_operation;
cin >> num_operation;

for(int i = 1; i <= num_operation; i++) {
    int T, A, B;
    cin >> T >> A >> B;

    if (T != 0) {  // Add Operation
        Update(A, T);
        Update(B + 1, T);
    } else {  // GCD Query
        cout << abs(__gcd(GetSum(1, A), GetGCD(A + 1, B))) << '\n';
    }
}
```
### Efficient Calculation

What we can see here is that
N is up to 1e5, while Q is up to 1e5 too, which are very large.
Therefore, Update, GetSum, and GetGCD cannot normally calculate in O(n).
For me, I used Segment tree to help me calculate faster.
Here is my AC code: https://ideone.com/wGPCWc

This is the end of this problem.
Enjoy!

// Chasing your light,at 25:00
