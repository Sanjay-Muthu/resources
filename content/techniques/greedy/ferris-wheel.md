---
draft: false
title: 'Ferris wheel'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Ferris Wheel"
weight: 4
---

{{< problem "cses-ferris-wheel" >}}

## Abridged problem statement

Given an array $a$ and an integer $x$, partition the elements of $a$ into as few groups as possible. Each group may contain at most two elements, and the sum of its elements must not exceed $x$. Determine the minimum number of groups.

## Solution

**Note:** we use the same technique introduced in [this article]({{< ref "/techniques/greedy/apartments.md" >}}), which may be considered a prerequisite.

Once again, let us employ the extremal principle. Given $a$, assuming $|a| > 1$, consider $m=\min\{a\}$ and $M=\max\{a\}$.

If $m+M > x$, we are forced to create a separate group for $M$—it cannot pair with any other element if it can't pair with the smallest!

If $m+M \le x$, must we pair $M$ with $m$? 

Consider the contrary—that is—consider the case when $M$ is paired with some other element $e$. We have two subcases:

### Case #1: $m$ is unpaired

(symmetric to $M$ is unpaired, which has been omitted)

In this case, we can simply replace $e$ with $m$, keeping the number of groups the same.

### Case #2: $m$ is paired with some other $f$

We know that $M+e \le x$ (turns out we don't even need to use $m+f \le x$).

Since $m \le e$ and $M+e \le x$, $M+m \le x$ as well. But is $e+f \le x$?

Yes, because $e+M \le x$ and $f \le M$ implies $e+f \le x$.

So, yes, pairing $m$ with $M$ really just works. This may seem somewhat magical at first, but we're actually doing almost the exact same thing as in [apartments]({{< ref "/techniques/greedy/apartments.md" >}}).

<figure>
  <div class="pairing-diagrams">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 465" role="img" aria-label="Before: m, e, f, M; pairs m–f and e–M">
      <defs>
        <marker id="before-arrow-1" markerUnits="userSpaceOnUse" viewBox="0 0 16 16" refX="14" refY="8" markerWidth="16" markerHeight="16" orient="auto-start-reverse">
          <path d="M 2 2 L 14 8 L 2 14" fill="none" stroke="#16c95a" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </marker>
      </defs>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="70" font-style="italic" text-anchor="middle">
        <text x="90" y="275">m</text><text x="420" y="275">e</text>
        <text x="700" y="275">f</text><text x="910" y="275">M</text>
      </g>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="38" text-anchor="middle">
        <text x="250" y="268">· · ·</text><text x="560" y="268">· · ·</text><text x="805" y="268">· · ·</text>
      </g>
      <g fill="none" stroke="#16c95a" stroke-width="4" stroke-linecap="round" marker-start="url(#before-arrow-1)" marker-end="url(#before-arrow-1)">
        <path d="M 90 207 C 90 11, 700 11, 700 207"/>
        <path d="M 420 312 C 420 469, 910 469, 910 312"/>
      </g>
    </svg>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 465" role="img" aria-label="Before: m, f, e, M; pairs m–f and e–M">
      <defs>
        <marker id="before-arrow-2" markerUnits="userSpaceOnUse" viewBox="0 0 16 16" refX="14" refY="8" markerWidth="16" markerHeight="16" orient="auto-start-reverse">
          <path d="M 2 2 L 14 8 L 2 14" fill="none" stroke="#16c95a" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </marker>
      </defs>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="70" font-style="italic" text-anchor="middle">
        <text x="90" y="275">m</text><text x="420" y="275">f</text>
        <text x="700" y="275">e</text><text x="910" y="275">M</text>
      </g>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="38" text-anchor="middle">
        <text x="250" y="268">· · ·</text><text x="560" y="268">· · ·</text><text x="805" y="268">· · ·</text>
      </g>
      <g fill="none" stroke="#16c95a" stroke-width="4" stroke-linecap="round" marker-start="url(#before-arrow-2)" marker-end="url(#before-arrow-2)">
        <path d="M 90 207 C 90 101, 420 101, 420 207"/>
        <path d="M 700 312 C 700 379, 910 379, 910 312"/>
      </g>
    </svg>
  </div>
  <figcaption>Before...</figcaption>
</figure>

<figure>
  <div class="pairing-diagrams">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 465" role="img" aria-label="After: m, e, f, M; old pairs m–f and e–M crossed out, new pairs m–M and e–f">
      <defs>
        <marker id="old-arrow-1" markerUnits="userSpaceOnUse" viewBox="0 0 16 16" refX="14" refY="8" markerWidth="16" markerHeight="16" orient="auto-start-reverse">
          <path d="M 2 2 L 14 8 L 2 14" fill="none" stroke="#ef4444" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </marker>
        <marker id="new-arrow-1" markerUnits="userSpaceOnUse" viewBox="0 0 16 16" refX="14" refY="8" markerWidth="16" markerHeight="16" orient="auto-start-reverse">
          <path d="M 2 2 L 14 8 L 2 14" fill="none" stroke="#20e875" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </marker>
      </defs>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="70" font-style="italic" text-anchor="middle">
        <text x="90" y="275">m</text><text x="420" y="275">e</text>
        <text x="700" y="275">f</text><text x="910" y="275">M</text>
      </g>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="38" text-anchor="middle">
        <text x="250" y="268">· · ·</text><text x="560" y="268">· · ·</text><text x="805" y="268">· · ·</text>
      </g>
      <!-- Old pairs -->
      <g fill="none" stroke="#ef4444" stroke-width="4" stroke-dasharray="7 10" stroke-linecap="round" marker-start="url(#old-arrow-1)" marker-end="url(#old-arrow-1)">
        <path d="M 90 207 C 90 11, 700 11, 700 207"/>
        <path d="M 420 312 C 420 469, 910 469, 910 312"/>
      </g>
      <g fill="none" stroke="#20e875" stroke-width="4" stroke-linecap="round" marker-start="url(#new-arrow-1)" marker-end="url(#new-arrow-1)">
        <path d="M 90 207 C 90 -50, 910 -50, 910 207"/>
        <path d="M 420 312 C 420 402, 700 402, 700 312"/>
      </g>
    </svg>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 465" role="img" aria-label="After: m, f, e, M; old pairs m–f and e–M crossed out, new pairs m–M and f–e">
      <defs>
        <marker id="old-arrow-2" markerUnits="userSpaceOnUse" viewBox="0 0 16 16" refX="14" refY="8" markerWidth="16" markerHeight="16" orient="auto-start-reverse">
          <path d="M 2 2 L 14 8 L 2 14" fill="none" stroke="#ef4444" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </marker>
        <marker id="new-arrow-2" markerUnits="userSpaceOnUse" viewBox="0 0 16 16" refX="14" refY="8" markerWidth="16" markerHeight="16" orient="auto-start-reverse">
          <path d="M 2 2 L 14 8 L 2 14" fill="none" stroke="#20e875" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </marker>
      </defs>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="70" font-style="italic" text-anchor="middle">
        <text x="90" y="275">m</text><text x="420" y="275">f</text>
        <text x="700" y="275">e</text><text x="910" y="275">M</text>
      </g>
      <g fill="#737373" font-family="Georgia, 'Times New Roman', serif" font-size="38" text-anchor="middle">
        <text x="250" y="268">· · ·</text><text x="560" y="268">· · ·</text><text x="805" y="268">· · ·</text>
      </g>
      <g fill="none" stroke="#ef4444" stroke-width="4" stroke-dasharray="7 10" stroke-linecap="round" marker-start="url(#old-arrow-2)" marker-end="url(#old-arrow-2)">
        <path d="M 90 207 C 90 101, 420 101, 420 207"/>
        <path d="M 700 312 C 700 379, 910 379, 910 312"/>
      </g>
      <g fill="none" stroke="#20e875" stroke-width="4" stroke-linecap="round" marker-start="url(#new-arrow-2)" marker-end="url(#new-arrow-2)">
        <path d="M 90 207 C 90 -50, 910 -50, 910 207"/>
        <path d="M 420 312 C 420 402, 700 402, 700 312"/>
      </g>
    </svg>
  </div>

  <figcaption>...and after!</figcaption>
</figure>

<style>
  .pairing-diagrams {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }

  .pairing-diagrams svg {
    display: block;
    width: 100%;
  }

  .pairing-diagrams svg:first-child {
    box-sizing: border-box;
    border-right: 1px solid #737373;
    padding-right: 1rem;
  }

  @media (max-width: 700px) {
    .pairing-diagrams {
      grid-template-columns: 1fr;
    }

    .pairing-diagrams svg:first-child {
      border-right: 0;
      border-bottom: 1px solid #737373;
      padding-right: 0;
      padding-bottom: 1rem;
    }
  }
</style>

## Implementation

We maintain a pointer at the front and back.

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int n, x;
  cin >> n >> x;
  vector<int> a(n);
  for (int &i : a) {
    cin >> i;
  }
  
  sort(a.begin(), a.end());
  int i = 0, j = a.size() - 1;
  int ans = 0;
  while (i <= j) {
    ans++;
    if (a[i] + a[j] <= x) {
      i++, j--;
    } else {
      j--;
    }
  }

  cout << ans << '\n';
}
```