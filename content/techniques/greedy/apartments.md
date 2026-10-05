---
draft: false
title: 'Apartments'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Apartments"
weight: 3
---

{{< problem "cses-apartments" >}}

## Abridged problem statement

Given two multisets $A$ and $B$, and a number $k$, find the size of a maximum matching given that two elements $a \in A, b \in B$ can be matched iff $|a-b| \le k$.

That is, form as many pairs $(a,b)$ with $a \in A, b \in B$ as possible, using every element at most once.

You should take a minute to think about why the original problem statement is equivalent to this abridged version. 

## Solution

Again, with most greedy problems, there's really no good way to begin. However, there are things we can try. Allow me to introduce you to another tool you should add to your toolkit: the [extremal principle](https://brilliant.org/wiki/extremal-principle/).

The idea behind this problem-solving technique is simple–a complicated problem may be simpler to think about if we consider 'extremal' cases—that is, cases involving the minimum and maximum elements. 

Let's think about the minimum element across both $A$ and $B$, call this $m$, and let's assume that $m \in A$. Maybe our two sets look something like this:

<figure class="diagram"><svg viewBox="0 0 800 165" aria-labelledby="caption"><g fill="#1677ff"><circle cx="70" cy="85" r="9"/><circle cx="400" cy="85" r="9"/><circle cx="495" cy="85" r="9"/><circle cx="615" cy="85" r="9"/><circle cx="745" cy="85" r="9"/></g><g fill="#f5a000"><circle cx="300" cy="125" r="9"/><circle cx="430" cy="125" r="9"/><circle cx="550" cy="125" r="9"/><circle cx="690" cy="125" r="9"/></g><g stroke="currentColor" stroke-width="1.5"><line x1="70" y1="44" x2="300" y2="44"/><line x1="70" y1="37" x2="70" y2="51"/><line x1="300" y1="37" x2="300" y2="51"/></g><text x="185" y="30" fill="currentColor" font-family="system-ui,sans-serif" font-size="17" text-anchor="middle">&gt; k</text></svg><figcaption id="caption">The minimum elements of the two sets are more than <em>k</em> apart.</figcaption></figure><style>.diagram{max-width:800px;margin:2rem auto;text-align:center}.diagram svg{display:block;width:100%}.diagram figcaption{margin-top:.4rem;color:#888;font-size:.9rem}</style>

Here, the blue circles denote elements in $A$ (the leftmost blue element is $m$), and the orange circles denote elements in $B$.

In this case, we cannot match $m$ with any element in $B$. Thus, it seems reasonable to assume that the answer will remain unchanged if we remove $m$ from $A$?

$$
A \leftarrow A \setminus \{m\}
$$

But what if the distance between $m_a:=\min\{A\}$ and $m_b:=\min\{B\}$ was $\le k$?

<figure class="diagram"><svg viewBox="0 0 800 165" aria-labelledby="caption"><g fill="#1677ff"><circle cx="190" cy="85" r="9"/><circle cx="400" cy="85" r="9"/><circle cx="495" cy="85" r="9"/><circle cx="615" cy="85" r="9"/><circle cx="745" cy="85" r="9"/></g><g fill="#f5a000"><circle cx="300" cy="125" r="9"/><circle cx="430" cy="125" r="9"/><circle cx="550" cy="125" r="9"/><circle cx="690" cy="125" r="9"/></g><g stroke="currentColor" stroke-width="1.5"><line x1="190" y1="44" x2="300" y2="44"/><line x1="190" y1="37" x2="190" y2="51"/><line x1="300" y1="37" x2="300" y2="51"/></g><text x="245" y="30" fill="currentColor" font-family="system-ui,sans-serif" font-size="17" text-anchor="middle">≤ k</text></svg><figcaption id="caption">The minimum elements of the two sets are at most <em>k</em> apart.</figcaption></figure><style>.diagram{max-width:800px;margin:2rem auto;text-align:center}.diagram svg{display:block;width:100%}.diagram figcaption{margin-top:.4rem;color:#888;font-size:.9rem}</style>

We could then match $m_a$ with $m_b$, but should we?

## An optimal matching?

Suppose we already have an optimal matching. If it is possible to modify this matching so that it:

* Remains valid
* Now includes $(m_a, m_b)$
* Has the same (or better) cardinality as before

then we will have successfully proven that there exists **an** optimal matching containing $(m_a, m_b)$. It remains to be seen whether or not this is possible.

To begin with, consider this extremely simple case:

<figure class="diagram"><svg viewBox="0 0 800 165" aria-labelledby="caption"><g fill="none" stroke="#16c95a" stroke-width="2.5"><line x1="400" y1="85" x2="430" y2="125"/><line x1="550" y1="125" x2="615" y2="85"/><line x1="690" y1="125" x2="745" y2="85"/></g><g fill="#1677ff"><circle cx="190" cy="85" r="9"/><circle cx="400" cy="85" r="9"/><circle cx="495" cy="85" r="9"/><circle cx="615" cy="85" r="9"/><circle cx="745" cy="85" r="9"/></g><g fill="#f5a000"><circle cx="300" cy="125" r="9"/><circle cx="430" cy="125" r="9"/><circle cx="550" cy="125" r="9"/><circle cx="690" cy="125" r="9"/></g></svg><figcaption id="caption"><em>m</em><sub>a</sub> and <em>m</em><sub>b</sub> are not matched with anything.</figcaption></figure><style>.diagram{max-width:800px;margin:2rem auto;text-align:center}.diagram svg{display:block;width:100%}.diagram figcaption{margin-top:.4rem;color:#888;font-size:.9rem}</style>

We can simply match $m_a$ with $m_b$ without making any other changes. Next, consider this case:

<figure class="diagram"><svg viewBox="0 0 800 165" aria-labelledby="caption"><g fill="none" stroke="#16c95a" stroke-width="2.5"><line x1="300" y1="125" x2="400" y2="85"/><line x1="495" y1="85" x2="550" y2="125"/><line x1="690" y1="125" x2="745" y2="85"/></g><g fill="#1677ff"><circle cx="190" cy="85" r="9"/><circle cx="400" cy="85" r="9"/><circle cx="495" cy="85" r="9"/><circle cx="615" cy="85" r="9"/><circle cx="745" cy="85" r="9"/></g><g fill="#f5a000"><circle cx="300" cy="125" r="9"/><circle cx="430" cy="125" r="9"/><circle cx="550" cy="125" r="9"/><circle cx="690" cy="125" r="9"/></g></svg><figcaption id="caption"><em>m</em><sub>b</sub> is matched with an element other than <em>m</em><sub>a</sub>, which is not matched.</figcaption></figure><style>.diagram{max-width:800px;margin:2rem auto;text-align:center}.diagram svg{display:block;width:100%}.diagram figcaption{margin-top:.4rem;color:#888;font-size:.9rem}</style>

We can include $(m_a, m_b)$ by removing the existing matching $m_b$ is a part of and matching it with $m_a$ instead. The number of matchings remains unchanged.

<figure class="diagram"><svg viewBox="0 0 800 165" aria-labelledby="caption"><g fill="none" stroke-width="2.5"><line x1="300" y1="125" x2="400" y2="85" stroke="#ff4d4f" stroke-dasharray="6 5"/><line x1="190" y1="85" x2="300" y2="125" stroke="#20e875" stroke-width="3"/><line x1="495" y1="85" x2="550" y2="125" stroke="#16c95a"/><line x1="690" y1="125" x2="745" y2="85" stroke="#16c95a"/></g><g fill="#1677ff"><circle cx="190" cy="85" r="9"/><circle cx="400" cy="85" r="9"/><circle cx="495" cy="85" r="9"/><circle cx="615" cy="85" r="9"/><circle cx="745" cy="85" r="9"/></g><g fill="#f5a000"><circle cx="300" cy="125" r="9"/><circle cx="430" cy="125" r="9"/><circle cx="550" cy="125" r="9"/><circle cx="690" cy="125" r="9"/></g></svg><figcaption id="caption">Replace the previous match containing <em>m</em><sub>B</sub> with the pair (<em>m</em><sub>A</sub>, <em>m</em><sub>B</sub>).</figcaption></figure><style>.diagram{max-width:800px;margin:2rem auto;text-align:center}.diagram svg{display:block;width:100%}.diagram figcaption{margin-top:.4rem;color:#888;font-size:.9rem}</style>

The most interesting case is when both $m_a$ and $m_b$ are matched:

<figure class="diagram"><svg viewBox="0 0 800 165" aria-labelledby="caption"><g fill="none" stroke="#16c95a" stroke-width="2.5"><line x1="300" y1="125" x2="400" y2="85"/><line x1="190" y1="85" x2="430" y2="125"/><line x1="495" y1="85" x2="550" y2="125"/><line x1="690" y1="125" x2="745" y2="85"/></g><g fill="#1677ff"><circle cx="190" cy="85" r="9"/><circle cx="400" cy="85" r="9"/><circle cx="495" cy="85" r="9"/><circle cx="615" cy="85" r="9"/><circle cx="745" cy="85" r="9"/></g><g fill="#f5a000"><circle cx="300" cy="125" r="9"/><circle cx="430" cy="125" r="9"/><circle cx="550" cy="125" r="9"/><circle cx="690" cy="125" r="9"/></g></svg><figcaption id="caption">The matching contains a criss-cross.</figcaption></figure><style>.diagram{max-width:800px;margin:2rem auto;text-align:center}.diagram svg{display:block;width:100%}.diagram figcaption{margin-top:.4rem;color:#888;font-size:.9rem}</style>

I claim that the following replacement is possible:

<figure class="diagram"><svg viewBox="0 0 800 165" aria-labelledby="caption"><g fill="none" stroke-width="2.5"><line x1="300" y1="125" x2="400" y2="85" stroke="#ff4d4f" stroke-dasharray="6 5"/><line x1="190" y1="85" x2="430" y2="125" stroke="#ff4d4f" stroke-dasharray="6 5"/><line x1="190" y1="85" x2="300" y2="125" stroke="#20e875" stroke-width="3"/><line x1="400" y1="85" x2="430" y2="125" stroke="#20e875" stroke-width="3"/><line x1="495" y1="85" x2="550" y2="125" stroke="#16c95a"/><line x1="690" y1="125" x2="745" y2="85" stroke="#16c95a"/></g><g fill="#1677ff"><circle cx="190" cy="85" r="9"/><circle cx="400" cy="85" r="9"/><circle cx="495" cy="85" r="9"/><circle cx="615" cy="85" r="9"/><circle cx="745" cy="85" r="9"/></g><g fill="#f5a000"><circle cx="300" cy="125" r="9"/><circle cx="430" cy="125" r="9"/><circle cx="550" cy="125" r="9"/><circle cx="690" cy="125" r="9"/></g></svg><figcaption id="caption">The crossed pairs can be replaced without changing the matching’s cardinality.</figcaption></figure><style>.diagram{max-width:800px;margin:2rem auto;text-align:center}.diagram svg{display:block;width:100%}.diagram figcaption{margin-top:.4rem;color:#888;font-size:.9rem}</style>

The proof is straightforward. Let the two matched pairs be $(m_a,B_1)$ and $(A_1,m_b)$.

We already know that $(m_a,m_b)$ is valid because $|m_a-m_b|\le k$. We only need to prove that $(A_1,B_1)$ is also valid.

Since $m_a$ and $m_b$ are the minimum elements of their respective sets,

$$
m_a\le A_1
\qquad\text{and}\qquad
m_b\le B_1.
$$

Now consider the relative order of $A_1$ and $B_1$:

* If $B_1<A_1$ (this is what's depicted in the above diagram), then

$$
A_1-B_1\le A_1-m_b\le k,
$$

because $(A_1,m_b)$ was a valid pair.

* If $A_1\le B_1$, then

$$
B_1-A_1\le B_1-m_a\le k,
$$

because $(m_a,B_1)$ was a valid pair.

Therefore, $|A_1-B_1|\le k$ in either case. Thus, we can replace the pairs $(m_a,B_1)$ and $(A_1,m_b)$ with $(m_a,m_b)$ and $(A_1,B_1)$ without changing the matching’s cardinality.

If this is your first time solving this problem, keep in mind that this is a hard and novel idea, and you may benefit from a re-read. Anyway, with all that work, we've concluded that matching $(m_a, m_b)$ (when we can) is always optimal. Then, we set

$$
A \leftarrow A \setminus \{m_a\}
$$
and 
$$
B \leftarrow B \setminus \{m_b\}
$$

When we can't, it's safe to discard the smaller of the two. 

## Implementation

A simple two-pointer approach suffices.

```cpp
#include <bits/stdc++.h>
 
using namespace std;
 
int main() {
  int n, m, k;
  cin >> n >> m >> k;
 
  vector<int> a(n), b(m);
  for (int &i : a) {
    cin >> i;
  }
  for (int &i : b) {
    cin >> i;
  }
 
  sort(a.begin(), a.end());
  sort(b.begin(), b.end());
 
  int ans = 0;
 
  int i = 0, j = 0;
  while (i < int(a.size()) && j < int(b.size())) {
    if (abs(a[i] - b[j]) > k) {
      if (a[i] < b[j]) {
        i++;
      } else {
        j++;
      }
      continue;
    }
    ans++;
    i++, j++;
  }
 
  cout << ans << '\n';
}
```