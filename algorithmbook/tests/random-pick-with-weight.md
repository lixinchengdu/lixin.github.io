# 912. Random Pick with Weight

* *Difficulty: Medium*

* *Topics: Binary Search, Random*

* *Similar Questions:*

  * [Random Pick Index](./tests/random-pick-with-weight.md)

  * [Random Pick with Blacklist](./tests/random-pick-with-weight.md)

  * [Random Point in Non-overlapping Rectangles](./tests/random-pick-with-weight.md)

## Problem:

<p>Given an array <code>w</code> of positive integers, where <code>w[i]</code> describes the weight of index <code>i</code>,&nbsp;write a function <code>pickIndex</code>&nbsp;which randomly&nbsp;picks an index&nbsp;in proportion&nbsp;to its weight.</p>

<p>Note:</p>

<ol>
	<li><code>1 &lt;= w.length &lt;= 10000</code></li>
	<li><code>1 &lt;= w[i] &lt;= 10^5</code></li>
	<li><code>pickIndex</code>&nbsp;will be called at most <code>10000</code> times.</li>
</ol>

<p><strong>Example 1:</strong></p>

<pre>
<strong>Input: 
</strong><span id="example-input-1-1">[&quot;Solution&quot;,&quot;pickIndex&quot;]
</span><span id="example-input-1-2">[[[1]],[]]</span>
<strong>Output: </strong><span id="example-output-1">[null,0]</span>
</pre>

<div>
<p><strong>Example 2:</strong></p>

<pre>
<strong>Input: 
</strong><span id="example-input-2-1">[&quot;Solution&quot;,&quot;pickIndex&quot;,&quot;pickIndex&quot;,&quot;pickIndex&quot;,&quot;pickIndex&quot;,&quot;pickIndex&quot;]
</span><span id="example-input-2-2">[[[1,3]],[],[],[],[],[]]</span>
<strong>Output: </strong><span id="example-output-2">[null,0,1,1,1,0]</span></pre>
</div>

<p><strong>Explanation of Input Syntax:</strong></p>

<p>The input is two lists:&nbsp;the subroutines called&nbsp;and their&nbsp;arguments.&nbsp;<code>Solution</code>&#39;s&nbsp;constructor has one argument, the&nbsp;array <code>w</code>. <code>pickIndex</code> has no arguments.&nbsp;Arguments&nbsp;are&nbsp;always wrapped with a list, even if there aren&#39;t any.</p>

## Solutions:

```c++
class Solution {
public:
    Solution(vector<int> w) {
        int n = w.size();
        prefixSum.resize(n);
        prefixSum[0] = w[0];
        for (int i = 1; i < n; ++i) {
            prefixSum[i] = prefixSum[i-1] + w[i];
        }
    }
    
    int pickIndex() {
        int max = prefixSum.back();
        int randNum = rand()%max + 1;
        
        auto it = lower_bound(prefixSum.begin(), prefixSum.end(), randNum);
        return it - prefixSum.begin();
    }
    
    vector<long long> prefixSum;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(w);
 * int param_1 = obj.pickIndex();
 */
```
