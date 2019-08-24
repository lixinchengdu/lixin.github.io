# 38. Count and Say

* *Difficulty: Easy*

* *Topics: String*

* *Similar Questions:*

  * [Encode and Decode Strings](./tests/count-and-say.md)

  * [String Compression](./tests/count-and-say.md)

## Problem:

<p>The count-and-say sequence is the sequence of integers with the first five terms as following:</p>

<pre>
1.     1
2.     11
3.     21
4.     1211
5.     111221
</pre>

<p><code>1</code> is read off as <code>&quot;one 1&quot;</code> or <code>11</code>.<br />
<code>11</code> is read off as <code>&quot;two 1s&quot;</code> or <code>21</code>.<br />
<code>21</code> is read off as <code>&quot;one 2</code>, then <code>one 1&quot;</code> or <code>1211</code>.</p>

<p>Given an integer <i>n</i>&nbsp;where 1 &le; <em>n</em> &le; 30, generate the <i>n</i><sup>th</sup> term of the count-and-say sequence.</p>

<p>Note: Each term of the sequence of integers will be represented as a string.</p>

<p>&nbsp;</p>

<p><b>Example 1:</b></p>

<pre>
<b>Input:</b> 1
<b>Output:</b> &quot;1&quot;
</pre>

<p><b>Example 2:</b></p>

<pre>
<b>Input:</b> 4
<b>Output:</b> &quot;1211&quot;</pre>

## Solutions:

```c++
class Solution {
public:
    string countAndSay(int n) {
        if (n == 1) return "1";
        string lastTerm = countAndSay(n-1);
        string ret;
        char lastChar = lastTerm[0];
        int cnt = 0;
        for (auto c : lastTerm) {
            if (c == lastChar)  ++cnt;
            else {
                ret.push_back('0' + cnt);
                ret.push_back(lastChar);
                lastChar = c;
                cnt = 1;
            }
        }
        ret.push_back('0' + cnt);
        ret.push_back(lastChar);
        return ret;
    }
};
```
