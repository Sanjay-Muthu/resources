---
draft: false
title: "Number Spiral"
editorial:
  platform: "CSES"
  category: "Introductory Problems"
  name: "Number Spiral"
weight: 1
---

*Editorial written by Sri Vidya Sundar.*

{{< problem "cses-number-spiral" >}}

## Abridged problem statement

Given a grid with numbers filled in the form of an outward spiral, find the number that is present in the cell $(x,y)$.

## Observation

Consider a square from the top left corner to a cell $(a,a)$. This square contains all the numbers from $1$ to $a^2$. For example, a square starting at cell $(1,1)$ and ending at cell $(3,3)$ has values from $1$ to $9$.

<style>
.number-spiral-methods{display:flex;flex-wrap:wrap;justify-content:center;align-items:flex-start;gap:24px;margin:1rem auto;font-family:sans-serif}
.number-spiral-figure{display:flex;flex-direction:column;align-items:center;width:fit-content;margin:0 auto}
.number-spiral-grid{position:relative;width:300px;height:300px;text-align:center;color:black;background:transparent}
.number-spiral-cells{display:grid;grid-template-columns:repeat(5,1fr);grid-template-rows:repeat(5,1fr);gap:6px;width:300px;height:300px;box-sizing:border-box;padding:6px}
.number-spiral-cells div{display:flex;align-items:center;justify-content:center;font-weight:bold;font-size:1.2rem}
.number-spiral-blue{background:#2196F3}
.number-spiral-gray{background:#E0E0E0}
.number-spiral-arrow{position:absolute;inset:0;width:300px;height:300px;pointer-events:none;z-index:1}
.number-spiral-figure figcaption{margin-top:.75rem;text-align:center}
</style>

<div class="number-spiral-grid" style="margin:0 auto">
<div class="number-spiral-cells">
<div class="number-spiral-blue">1</div>
<div class="number-spiral-blue">2</div>
<div class="number-spiral-blue">9</div>
<div class="number-spiral-gray">10</div>
<div class="number-spiral-gray">25</div>
<div class="number-spiral-blue">4</div>
<div class="number-spiral-blue">3</div>
<div class="number-spiral-blue">8</div>
<div class="number-spiral-gray">11</div>
<div class="number-spiral-gray">24</div>
<div class="number-spiral-blue">5</div>
<div class="number-spiral-blue">6</div>
<div class="number-spiral-blue">7</div>
<div class="number-spiral-gray">12</div>
<div class="number-spiral-gray">23</div>
<div class="number-spiral-gray">16</div>
<div class="number-spiral-gray">15</div>
<div class="number-spiral-gray">14</div>
<div class="number-spiral-gray">13</div>
<div class="number-spiral-gray">22</div>
<div class="number-spiral-gray">17</div>
<div class="number-spiral-gray">18</div>
<div class="number-spiral-gray">19</div>
<div class="number-spiral-gray">20</div>
<div class="number-spiral-gray">21</div>
</div>
</div>

## Solution

Now let us divide the grid into squares starting from the top left corner.

Once the square of side length $\max(x,y)-1$ has been filled, all numbers up to $(\max(x,y)-1)^2$ have already been used. After this, the spiral fills the next layer in one of two ways:

<div class="number-spiral-methods">
<figure class="number-spiral-figure">
<div class="number-spiral-grid">
<div class="number-spiral-cells">
<div class="number-spiral-blue">1</div>
<div class="number-spiral-blue">2</div>
<div class="number-spiral-blue">9</div>
<div class="number-spiral-gray">10</div>
<div class="number-spiral-gray">25</div>
<div class="number-spiral-blue">4</div>
<div class="number-spiral-blue">3</div>
<div class="number-spiral-blue">8</div>
<div class="number-spiral-gray">11</div>
<div class="number-spiral-gray">24</div>
<div class="number-spiral-blue">5</div>
<div class="number-spiral-blue">6</div>
<div class="number-spiral-blue">7</div>
<div class="number-spiral-gray">12</div>
<div class="number-spiral-gray">23</div>
<div class="number-spiral-gray">16</div>
<div class="number-spiral-gray">15</div>
<div class="number-spiral-gray">14</div>
<div class="number-spiral-gray">13</div>
<div class="number-spiral-gray">22</div>
<div class="number-spiral-gray">17</div>
<div class="number-spiral-gray">18</div>
<div class="number-spiral-gray">19</div>
<div class="number-spiral-gray">20</div>
<div class="number-spiral-gray">21</div>
</div>
<svg class="number-spiral-arrow" viewBox="0 0 300 300">
<defs>
<marker id="arrowhead-case-1" markerWidth="6" markerHeight="6" refX="2" refY="3" orient="auto">
<path d="M 0 0 L 6 3 L 0 6 Z" fill="#1565C0" fill-opacity="0.62"/>
</marker>
</defs>
<path d="M 209 32 L 209 209 L 32 209" fill="none" stroke="#1565C0" stroke-width="4.5" stroke-opacity="0.62" marker-end="url(#arrowhead-case-1)" stroke-linejoin="round"/>
</svg>
</div>
<figcaption>Case (i)</figcaption>
</figure>
<figure class="number-spiral-figure">
<div class="number-spiral-grid">
<div class="number-spiral-cells">
<div class="number-spiral-blue">1</div>
<div class="number-spiral-blue">2</div>
<div class="number-spiral-blue">9</div>
<div class="number-spiral-blue">10</div>
<div class="number-spiral-gray">25</div>
<div class="number-spiral-blue">4</div>
<div class="number-spiral-blue">3</div>
<div class="number-spiral-blue">8</div>
<div class="number-spiral-blue">11</div>
<div class="number-spiral-gray">24</div>
<div class="number-spiral-blue">5</div>
<div class="number-spiral-blue">6</div>
<div class="number-spiral-blue">7</div>
<div class="number-spiral-blue">12</div>
<div class="number-spiral-gray">23</div>
<div class="number-spiral-blue">16</div>
<div class="number-spiral-blue">15</div>
<div class="number-spiral-blue">14</div>
<div class="number-spiral-blue">13</div>
<div class="number-spiral-gray">22</div>
<div class="number-spiral-gray">17</div>
<div class="number-spiral-gray">18</div>
<div class="number-spiral-gray">19</div>
<div class="number-spiral-gray">20</div>
<div class="number-spiral-gray">21</div>
</div>
<svg class="number-spiral-arrow" viewBox="0 0 300 300">
<defs>
<marker id="arrowhead-case-2" markerWidth="6" markerHeight="6" refX="2" refY="3" orient="auto">
<path d="M 0 0 L 6 3 L 0 6 Z" fill="#1565C0" fill-opacity="0.62"/>
</marker>
</defs>
<path d="M 32 268 L 268 268 L 268 32" fill="none" stroke="#1565C0" stroke-width="4.5" stroke-opacity="0.62" marker-end="url(#arrowhead-case-2)" stroke-linejoin="round"/>
</svg>
</div>
<figcaption>Case (ii)</figcaption>
</figure>
</div>

We use case (i) when the blue square has an odd side length, and case (ii) otherwise.

For simplicity, assume $x \ge y$. Then the already-filled square has side length $x-1$, so the last used number is $(x-1)^2$.

In case (i), we first move $x$ cells down, then move back $x-y$ cells. So the answer is

$$
(x-1)^2+x+(x-y)=(x-1)^2+2x-y.
$$

In case (ii), we move $y$ more cells, so the answer is

$$
(x-1)^2+y.
$$

The case $y > x$ is symmetric.

## Implementation

```cpp
#include <bits/stdc++.h>

using namespace std;

int32_t main() {
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);

  long long t;
  cin >> t;

  while (t--) {
    long long x, y;
    cin >> x >> y;

    if (x > y) {
      if (x % 2 == 1) {
        cout << (x - 1) * (x - 1) + y;
      } else {
        cout << (x - 1) * (x - 1) + 2 * x - y;
      }
    } else {
      if (y % 2 == 0) {
        cout << (y - 1) * (y - 1) + x;
      } else {
        cout << (y - 1) * (y - 1) + 2 * y - x;
      }
    }

    cout << '\n';
  }
}
```