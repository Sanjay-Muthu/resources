---
draft: false
title: 'SUPW'
editorial:
  platform: "ZCO"
  name: "SUPW"
---

*Editorial written by Ekansh Majumder.*

{{< problem "zco-supw" >}}

We are given $N$ SUPW activities on different days, each taking a specific number of minutes. We need to select days to perform these duties such that we minimize the total time spent. The constraint is that no student can go three days in a row without participating.

There is only one subtask, and the number of days $N$ can be up to $2 \cdot 10^5$. A brute-force approach trying all valid combinations of days would take exponential time and will definitely not pass the time limit constraints. 

## Why greedy fails

There are two natural greedy solutions one might come up with for this problem. Let's go over both.

The first approach is to strictly skip as many days as possible: skip day $0$ and day $1$, which forces you to do day $2$. Then skip day $3$ and day $4$, which forces you to do day $5$, and so on. The idea is to never perform the duty unless you are absolutely forced to. This fails because the problem asks us to minimize the *number of minutes* spent, not the *number of days* we work.

A test case where this fails is:

$$[1, 3, 100, 1, 1, 100]$$

The optimal solution here is to pick day $0$ and day $4$ (or day $0$ and day $3$), which gives us a total of $2$ minutes. But our greedy solution forces us to pick day $2$ and day $5$, giving us a massive cost of $200$. 

The second approach is to constantly pick the smallest element within a $3$ day range from your current position. 
For example, using the same test case:

$$[1, 3, 100, 1, 1, 100]$$

If we look for the immediate minimum, we might pick day $0$, day $1$, and day $3$. This gives us a cost of $5$, which is still not the correct answer. This fails because the optimal solution sometimes requires picking a slightly larger number early on to buy enough "reach" to skip massive numbers later. The greedy approach snaps to the immediate smallest number, restricting our future choices.

## Why DP?

A greedy algorithm chooses the locally optimal choice. It does not have the foresight to evaluate how a decision made on day $1$ changes the options available to us on day $4$. A greedy approach simply cannot consider the long term consequences of a choice.

We use DP to solve this problem because it fixes the exact flaw that greedy approaches have. It evaluates all valid paths without actually simulating every single exponential combination. Instead of just looking for the best immediate choice, DP calculates the absolute cheapest way to arrive at the current state.

The rule stating a student "cannot go three days in a row without any SUPW duty" translates to a simple constraint: out of any $3$ consecutive days, we must pick at least one day to work. 

Since the decision of whether to work on a particular day only depends on the choices we made in the immediate past (specifically, the last three days), this problem exhibits optimal substructure and overlapping subproblems, making it perfect for dynamic programming.

## The DP state

Let's define a state that helps us keep track of the minimum cost. 

Let $dp[i]$ denote the minimum time spent on SUPW duties up to day $i$, **given that we choose to work on day $i$**. For the first three days, we have no previous days to rely on to skip a valid block. If we choose to work on day $0$, day $1$, or day $2$, the minimum time spent up to that day is just the time required for that day itself.

```c++
vector<int> dp(n);
dp[0] = a[0];
dp[1] = a[1];
dp[2] = a[2];
```

Notice that we define $dp[i]$ under the strict condition that we choose to work on day $i$. We do this to avoid running into a wall while calculating $dp[i+1]$.

For example, let's say we define $dp[i]$ to be the minimum cost for the first $i$ days, regardless of whether we worked on day $i$ or not. With this state, we run into a wall when calculating $dp[i+1]$. Let's say we are on day $6$ and we know the best score for the first $5$ days is $10$ ($dp[5] = 10$). Now we ask if we can skip day $6$. We have no memory or way to know what the last day we picked was, so we cannot enforce the three day rule using this DP state in a simple way. 

> **Note:** This trick of forcing the state to end on a specific element is also used to solve the Longest Increasing Subsequence (LIS) problem using DP (refer to page 70 [here](https://cses.fi/book/book.pdf)).

There is also an alternative way to solve this using a 2D DP, where $dp[i][j]$ represents the minimum cost from day $i$ to the end of the term, given that $j$ is the number of consecutive days we are still allowed to skip. We will also go over this solution later in the editorial.

## Transition

If we perform a duty on day $i$, when could we have performed our last duty?

According to the three day rule, the previous duty must have been on day $i-1$, $i-2$, or $i-3$. If our last duty was on day $i-4$, we would have skipped days $i-3$, $i-2$, and $i-1$. This means we would have skipped $3$ consecutive days, which is invalid.

This gives us our transition:
$dp[i] = a[i] + \min(dp[i-1], dp[i-2], dp[i-3])$

```cpp
for (int i = 3; i < n; i++) {
    dp[i] = a[i] + min({dp[i-1], dp[i-2], dp[i-3]});
}
```

The final duty we perform must be on one of the last three days of the term (day $N-1$, $N-2$, or $N-3$). If we finished our last duty on day $N-4$, we would again break the rule by skipping the final three days. Therefore, the overall minimum cost is simply the minimum value among the last three elements of our DP array.

## Implementation

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  cin.tie(NULL);
  ios::sync_with_stdio(false);
  
  int n;
  cin >> n;
  
  vector<int> a(n);
  for (int i = 0; i < n; i++) {
    cin >> a[i]; 
  }
  
  // dp[i] stores the minimum time
  // given that we perform SUPW duty on day i 
  vector<int> dp(n);
  dp[0] = a[0];
  dp[1] = a[1];
  dp[2] = a[2];
  
  // Calculate the minimum cost
  for (int i = 3; i < n; i++) {
    dp[i] = a[i] + min({dp[i - 1], dp[i - 2], dp[i - 3]});
  }
  
  cout << min({dp[n - 1], dp[n - 2], dp[n - 3]}) << endl;
}
```

The time complexity of this solution is $\mathcal{O}(N)$ as we iterate through the array of days exactly once.


## Alternate solution

*Thanks to Avighna for providing me with this solution.*

Let $dp[i][j]$ denote the minimum time spent on SUPW duties from day $i$ to the end of the term, given that we are allowed to skip up to $j$ more consecutive days.

Because this approach calculates the cost from the current day to the end of the array, we will build our DP table backwards (from day $N$ down to day $0$).

The second dimension, $j$, can take only $3$ values: $0$, $1$, or $2$.

If $j = 2$, then we can safely skip the current day and the next day.
If $j = 1$, then we have already skipped one day, so we are allowed to skip one more.
If $j = 0$, then we are forced to work on this day.

At any day $i$, the choices we can make depend entirely on our skip allowance $j$.

### Case #1: we have no skips left

If $j = 0$, we have already skipped the maximum allowed consecutive days. We must perform duty on day $i$. 
The cost is the time for today ($a[i]$) plus the optimal cost for the remaining days ($dp[i + 1][2]$). Because we worked today, our skip allowance for tomorrow completely resets to the maximum: $2$.

```c++
if (j == 0) {
    dp[i][j] = a[i] + dp[i + 1][2];
    continue;
}
```

### Case #2: we have skips remaining

If we have an allowance of $1$ or $2$, we have a choice. We can either perform the duty or skip it. 
We want the minimum of these two options:

* **Skip today:** we add a cost of $0$ for today, move to day $i + 1$, and our skip allowance decreases by $1$ (becoming $j - 1$).
* **Work today:** we pay the cost for today ($a[i]$), move to day $i + 1$, and our skip allowance resets to full ($2$).

This gives us the transition:
$dp[i][j] = \min(dp[i + 1][j - 1], a[i] + dp[i + 1][2])$

The base case occurs when $i = N$. we have reached the end of the term, so the cost for any remaining days is $0$.

```c++
using ll = int64_t;

ll n;

ll a[N];
ll dp[N][3];

int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);

  cin >> n;
  for (ll i = 0; i < n; ++i) {
    cin >> a[i];
  }

  for (ll i = n; i >= 0; --i) {
    for (ll j = 0; j < 3; ++j) {
      if (i == n) {
        continue;
      }
      if (j == 0) {
        dp[i][j] = a[i] + dp[i + 1][2];
        continue;
      }
      dp[i][j] = min(dp[i + 1][j - 1], a[i] + dp[i + 1][2]);
    }
  }

  cout << dp[0][2] << "\n";
}
```