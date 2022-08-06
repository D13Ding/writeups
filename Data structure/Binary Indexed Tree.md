# Binary Indexed Tree

### **樹狀陣列**

- 功能：計算前綴和，比線段樹更節省記憶體；其初衷是解決資料壓縮裡的累積頻率（Cumulative Frequency）的計算問題。
- 時間複雜度：
    - 求任意序列和：*O*(log*n*)
    - 修該單點值：*O*(log*n*)
- 空間複雜度：*O*(*n*)
- 結構：
    - 產生兩陣列：
        1. $A$ 儲存各元素
        2. $BIT$ 儲存特定元素的總和，$BIT_i=\sum^i_{j=i-lowbit(i)+1}A_j$
        
        其中 $lowbit(i)$ 為 $i$ 轉為二進位後最後一個 $1$ 所代表的數值，$lowbit(12)=(1100)_{2}=4$ ，代表BIT[12]儲存的是 A[9:12] 的總和
        
        ![Untitled](https://github.com/D13Ding/writeups/blob/main/Data%20structure/img/Binary%2520Indexed%2520Tree%2520.jpeg)
        
- 實作：
    - $lowbit()$
        
        以 $14$ 為例，$14$ 的Bitwise NOT為$(0001)_2$，加 $1$ 後為$(0010)_2$，最後將 $(0010)_2 \text{ AND } (1110)_2=(10)_2$，  $lowbit(14)=2$  
        
        ```cpp
        int lowbit (int i){
        	return (~i + 1) & i;	
        }
        ```
        
    - 新增
        
        產生一個 BIT 陣列，儲存A陣列的前綴和
        
        ```cpp
        void New (int A[arraySize]){
        	for (int i = 1; i <= arraySize; i++){
                BIT[i] = A[i - 1];
                for (int j = i - lowbit(i); j < i-1 ; j++)
                    BIT[i] += A[j];
            }
        }
        ```
        
    - 修改
        
        當某一欄位的值被修改時，BIT中有包含道該元素的欄位皆須更新，假設A[i] 的值增加 d 
        
        ```cpp
        void edit(int i, int d){
            for (int j = i; j <= arraySize; j += lowbit(j))
                BIT[j] += d;
        }
        ```
        
    - 前綴和
        
        若要找A[0:k]的總和，從 BIT[k] 開始，透過 k 減去 lowbit(k) 找到不重疊的前段區間並加總
        
        ```cpp
        int sum (int k){
            int ans = 0;
            for (int i = k; i > 0; i -= lowbit(i))
                ans += BIT[i];
            return ans;
        }
        ```
        

參考資料：

1. [https://yuihuang.com/binary-indexed-tree/?utm_source=rss&utm_medium=rss&utm_campaign=binary-indexed-tree](https://yuihuang.com/binary-indexed-tree/?utm_source=rss&utm_medium=rss&utm_campaign=binary-indexed-tree)
2. [https://zh.wikipedia.org/zh-tw/树状数组](https://zh.wikipedia.org/zh-tw/%E6%A0%91%E7%8A%B6%E6%95%B0%E7%BB%84)
3. [https://hackmd.io/@wiwiho/cp-note/%2F%40wiwiho%2FCPN-binary-indexed-tree](https://hackmd.io/@wiwiho/cp-note/%2F%40wiwiho%2FCPN-binary-indexed-tree)
