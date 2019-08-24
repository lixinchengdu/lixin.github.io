# 218. The Skyline Problem

* *Difficulty: Hard*

* *Topics: Divide and Conquer, Heap, Binary Indexed Tree, Segment Tree, Line Sweep*

* *Similar Questions:*

  * [Falling Squares](./tests/the-skyline-problem.md)

## Problem:

<p>A city&#39;s skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are <b>given the locations and height of all the buildings</b> as shown on a cityscape photo (Figure A), write a program to <b>output the skyline</b> formed by these buildings collectively (Figure B).</p>
<a href="/static/images/problemset/skyline1.jpg" target="_blank"><img alt="Buildings" src="https://assets.leetcode.com/uploads/2018/10/22/skyline1.png" style="max-width: 45%; border-width: 0px; border-style: solid;" /> </a> <a href="/static/images/problemset/skyline2.jpg" target="_blank"> <img alt="Skyline Contour" src="https://assets.leetcode.com/uploads/2018/10/22/skyline2.png" style="max-width: 45%; border-width: 0px; border-style: solid;" /> </a>

<p>The geometric information of each building is represented by a triplet of integers <code>[Li, Ri, Hi]</code>, where <code>Li</code> and <code>Ri</code> are the x coordinates of the left and right edge of the ith building, respectively, and <code>Hi</code> is its height. It is guaranteed that <code>0 &le; Li, Ri &le; INT_MAX</code>, <code>0 &lt; Hi &le; INT_MAX</code>, and <code>Ri - Li &gt; 0</code>. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.</p>

<p>For instance, the dimensions of all buildings in Figure A are recorded as: <code>[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] </code>.</p>

<p>The output is a list of &quot;<b>key points</b>&quot; (red dots in Figure B) in the format of <code>[ [x1,y1], [x2, y2], [x3, y3], ... ]</code> that uniquely defines a skyline. <b>A key point is the left endpoint of a horizontal line segment</b>. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.</p>

<p>For instance, the skyline in Figure B should be represented as:<code>[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]</code>.</p>

<p><b>Notes:</b></p>

<ul>
	<li>The number of buildings in any input list is guaranteed to be in the range <code>[0, 10000]</code>.</li>
	<li>The input list is already sorted in ascending order by the left x position <code>Li</code>.</li>
	<li>The output list must be sorted by the x position.</li>
	<li>There must be no consecutive horizontal lines of equal height in the output skyline. For instance, <code>[...[2 3], [4 5], [7 5], [11 5], [12 7]...]</code> is not acceptable; the three lines of height 5 should be merged into one in the final output as such: <code>[...[2 3], [4 5], [12 7], ...]</code></li>
</ul>

## Solutions:

```c++
class Solution {
public:
    struct Line
    {
        int x;
        int height;
        bool up;
    };
    
    class LineCmp
    {
        public:
        bool operator() (Line& l1, Line&l2)
        {
            if (l1.x > l2.x)    return true;
            else if (l1.x == l2.x)
            {
                if (l1.up && !l2.up)    return false;
                if (!l1.up && l2.up)    return true;
                if (l1.up && l2.up) {return l1.height < l2.height;}
                if (!l1.up && !l2.up) {return l1.height > l2.height;}
            }
            else return false;
        }
    };
    

    vector<pair<int, int>> getSkyline(vector<vector<int>>& buildings) {
        priority_queue<Line, vector<Line>, LineCmp> lineQueue;
        multiset<int> heightSet;
        heightSet.insert(0);
        //heightQueue.push(0);
        vector <pair<int, int> > result;
        for (auto building: buildings)
        {
            Line l1;
            l1.x = building[0];
            l1.height = building[2];
            l1.up = true;
            lineQueue.push(l1);
            Line l2;
            l2.x = building[1];
            l2.height = building[2];
            l2.up = false;
            lineQueue.push(l2);
        }
        while(!lineQueue.empty())
        {
            Line l = lineQueue.top();
            lineQueue.pop();
            //cout << "x:" << l.x << endl;
            //cout << "up?:" << l.up << endl; 
            //cout << "line height" << l.height << endl;
            //cout << "height:" << heightQueue.top() << endl;
            if (l.up)
            {
                if ( l.height > *heightSet.rbegin())
                {
                    result.push_back(make_pair(l.x, l.height));
                }
                heightSet.insert(l.height);
            }
            else
            {
                //cout << "aha!" << endl;
                //cout << l.height << endl;
                //cout << heightQueue.top() << endl;
                if (l.height == *heightSet.rbegin())
                {
                    heightSet.erase(heightSet.find(l.height));
                    //cout << *heightSet.rbegin() << endl;
                    //if (heightQueue.empty())    result.push_back(make_pair(l.x, 0));
                    //else
                    {
                        if (l.height > *heightSet.rbegin())   result.push_back(make_pair(l.x, *heightSet.rbegin()));
                    }
                }
                else
                    heightSet.erase(heightSet.find(l.height));
            }
        }
        return result;
        
    }
};
```
