线段树例题及模版

本文通过总结线段树的各类例题来学习线段树的知识。

本文参考自codeforces中[ITMO Academy: pilot course](http://codeforces.com/edu/course/2)下的 [Segment Tree](http://codeforces.com/edu/course/2/lesson/4)章节，感谢codeforces提供如此高质量的教学。

<!--more-->

[toc]

# 1.包含点修改与区间查询的线段树

## 例题：

链接：[A - Segment Tree, part 1 - Codeforces](http://codeforces.com/edu/course/2/lesson/4/1/practice/contest/273169/problem/A)

<img src="https://gitee.com/wintermorning/img/raw/master/img/20210414085054.png" style="zoom:50%;" />

## 代码：

```c++
/*
 * author:wintermorning
 */

#include<vector>
#include<iostream>
using namespace std;

typedef long long LL;

struct segmentree{ 
    int size;
    int n;
    vector<LL> sums;

    void init(int n){ 
        size = 1;
        while(size<n)size<<=1;
        size<<=1;
        size++;
        this->n = n;
        sums.assign(size, 0LL);
    }

    void set(int pos,int val,int x,int lx,int rx){
        if(rx == lx){ 
            sums[x] = val;
            return ;
        }
        int mid = (lx+rx)>>1;
        if(pos<=mid) set(pos,val,x<<1,lx,mid);
        else set(pos,val,(x<<1)+1,mid+1,rx);
        sums[x] = sums[x<<1]+sums[(x<<1)+1];
    }
    void set(int pos,int val){ 
        set(pos,val,1,1,n);    
    }


    LL sum(int l,int r,int x,int lx,int rx){ 
        if(lx > r || l> rx) return 0;
        if(lx>=l && rx<= r) return sums[x];

        int mid = (lx+rx)>>1;
        LL s1 = sum(l,r,x<<1,lx,mid);
        LL s2 = sum(l,r,(x<<1)+1,mid+1,rx);
        return s1+s2;
    }

    LL sum(int l,int r){ 
        return sum(l,r,1,1,n);
    }

    void debug(int x,int lx,int rx){ 
        if(lx == rx){ 
            cout<<sums[x]<<" ";
            return ;
        }
        int mid = (lx+rx)>>1;
        debug(x*2,lx,mid);
        debug(x*2+1,mid+1,rx);
    }
};

int main(){ 
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    segmentree st;
    int n,m;
    cin>>n>>m;
    st.init(n);

    for(int i=1;i<=n;i++){ 
        int val;
        cin>>val;
        st.set(i,val);
    }

    while(m--){ 
        int op;
        cin>>op;
        if(op&1){ 
            int pos,val;
            cin>>pos>>val;
            st.set(pos+1,val);
        }
        else{ 
            int l,r;
            cin>>l>>r;
            cout<<st.sum(l+1,r)<<"\n";
        }
    }

}
```

