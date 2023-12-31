# 

## From local optima to global optima

### Distribution problem

455.Assign Cookies (Easy)
```

int findContentChildren(vector<int>& children, vector<int>& cookies) 
{ 
sort(children.begin(), children.end());
sort(cookies.begin(), cookies.end());
int child = 0, cookie = 0;
while (child < children.size() && cookie < cookies.size()) { 
if (children[child] <= cookies[cookie]) ++child; 
++cookie;
}
       return child;
}
```

You need to sort two vector from small to large. If the kid with lower desire meet the requirement, first assign to him. If can't meet, just to see next requirement.

### Interval problem

435.Non-overlapping Intervals (Medium)
```
int eraseOverlapIntervals(vector<vector<int>>& intervals) { if (intervals.empty()) {
return 0; }
int n = intervals.size();
sort(intervals.begin(), intervals.end(), [](vector<int> a, vector<int> b) {
          return a[1] < b[1];
       });
       int total = 0, prev = intervals[0][1];
       for (int i = 1; i < n; ++i) {
          if (intervals[i][0] < prev) {
              ++total;
          } 
          else {
              prev = intervals[i][1];
} }
       return total;
   }
```
Sequence the vector by end number because we first try to meet the smallest spilt out. In that case, the end number smaller, the better.
We set latter number as flag, if next vector begin number is over it, we don't need to spilt out but to check the next vector begin number.
