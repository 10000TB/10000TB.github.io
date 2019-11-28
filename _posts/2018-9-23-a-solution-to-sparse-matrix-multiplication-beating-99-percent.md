---
layout: "post"
title: "A solution to Sparse Matrix Multiplication beating 99 percent of submissions"
categories: code
tags: [leetcode, code, soluton]
---

The problem of interest is here - https://leetcode.com/problems/sparse-matrix-multiplication/description/


Here is my solution

(Also posted here - https://leetcode.com/problems/sparse-matrix-multiplication/discuss/76151/54ms-detailed-summary-of-easiest-java-solutions-beating-999/200413)


@yavinci  Talked about the "smart"/Easiest solution in his thread page ([here](https://discuss.leetcode.com/topic/30625/easiest-java-solution/12)), and most explanation down that page is not really touching why the smart solution is smart, So i am making this post to 1)explain in detail why the smart solution is smart and 2)make some improvements/tweaks on the smart solution code to show you which part is essential, 3) also i will briefly mention why [Sparse Matrix Manipulation](http://www.cs.cmu.edu/~scandal/cacm/node9.html) can help make some improvements on top of the smart solution.

a) Originally, the normal way to calculate the multiplication of two metrics A, and B is as follow:
        We take the the all values from the first line of A, and all values from the first column of B, and multiply the corresponding values and sum them up, the final sum is the value for the location of first column and first row in final result matrix.  Similarly, the value at [ i ][ j ] of result matrix C, which is C[ i ][ j ] is calculated as:
     
C[ i ][ j ] = A[ i ][0]*B[0][j]  +  A[i][1]*B[1][j] + A[i][2]*B[2][j] + ... A[i][K]*B[K][j]
( which is the sum of each multiplication of corresponding K values from row i of A and K values from column j of B )

```The Key is: if we calculate it this way, we finishing calculating the final value for the result matrix at once ```

Then the brute force solution is as follow:

```java

    public class Solution {
            public int[][] multiply(int[][] A, int[][] B) {
                int m = A.length, n = A[0].length, nB = B[0].length;
                int[][] C = new int[m][nB];

                for(int i = 0; i < m; i++) {
                        for (int j = 0; j < nB; j++) {
                            for(int k = 0; k < n; k++) {
                                 C[i][j] += A[i][k] * B[k][j];
                            }
                        }
                }
                return C;  
            }
    }

```

b) ****The smart solution****, the key part of smart solution is that: it does not calculate the final result at once, and it takes each value from A, and calculate and partial sum and accumulate it into the final spot:
For example, for each value A[i][k], if it is not zero, it will be used at most nB times ( n is B[0].length ), which can be illustrated as follow:
Generally for the following equation:
C[ i ][ j ] = A[ i ][0]*B[0][j]  +  A[i][1]*B[1][j] + A[i][2]*B[2][j] + ... A[i][k]*B[k][j] ....  A[i][K]*B[K][j]
j can be from 0 to nB, if we write all of them down, it will like following:
[For i from 0 to nB 
C[ i ][ 0 ]=A[ i ][0]*B[0][0]  +  A[i][1]*B[1][0] + A[i][2]*B[2][0] + ... A[i][k]B[k][0] ....  A[i][K]*B[K][0]
C[ i ][ 1 ]=A[ i ][0]*B[0][1]  +  A[i][1]*B[1][1] + A[i][2]*B[2][1] + ... A[i][k]B[k][0] ....  A[i][K]*B[K][1]
...
C[ i ][ nB ]=A[ i ][0]*B[0][nB]  +  A[i][1]*B[1][nB] + A[i][2]*B[2][nB] + ... A[i][k]B[k][nB] ....  A[i][K]*B[K][nB]

As you can see from above: for the same value A[i][k] from the first matrix, it will be used at most nB times if A[i][k] is not zero.  And the smart solution is taking advantage of that!!!, the smart solution can be described as:

For each value A[i][k] in matrix A, if it is not zero, we calculate A[i][k] * B[k][j] and accumulate it into C[ i ][ j ] (```Key part: the C[ i ][ j ] by now is not the final value in the result matrix !! Remember, in the brute force solution, the final value of C[i][j], takes sum of all multiplication values of K corresponding values from A and B? here C[ i ][ j ] is only sum of some multiplication values, NOT ALL until the program is done``` )

**BY NOW, it is very clear that, if the value A[ i ][ k ] from matrix is zero, we skip a For-loop- calculation, which is a loop iterating nB times, and this is the key part of why the smart solution is smart!!!**
 
The smart Solution Code is as follow:


```java
    public class Solution {
        public int[][] multiply(int[][] A, int[][] B) {
            int m = A.length, n = A[0].length, nB = B[0].length;
            int[][] C = new int[m][nB];

            for(int i = 0; i < m; i++) {
                for(int k = 0; k < n; k++) {
                    if (A[i][k] != 0) {
                        for (int j = 0; j < nB; j++) {
                            if (B[k][j] != 0) C[i][j] += A[i][k] * B[k][j];
                        }
                    }
                }
            }
            return C;   
        }
    }

```

( Credit:@yavinci , I am having a different version, so I am directly referencing the original version )

Based on the discussion above, the inner checking ( ```if (B[k][j] != 0) ```  )  is actually not necessary, because whether or not we have that check, we still iterate nB times, ( since the operation ```C[i][j] += A[i][k] * B[k][j];``` inside the if-check is O(1) time)

So the smart solution can also be written as follow by removing the check ( which is my version ):

```java

    public class Solution {
        public int[][] multiply(int[][] A, int[][] B) {
            int m = A.length, n = A[0].length, nB = B[0].length;
            int[][] C = new int[m][nB];

            for(int i = 0; i < m; i++) {
                for(int k = 0; k < n; k++) {
                    if (A[i][k] != 0) {
                        for (int j = 0; j < nB; j++) {
                            if (B[k][j] != 0) C[i][j] += A[i][k] * B[k][j];
                        }
                    }
                }
            }
            return C;   
        }
    }


```

c) "Sparse matrix manipultion" helps, if we compress the first sparse matrix into rows of lists( in each row list, it contains ( value, index ) pair ), we actually don't need to go over all values in a row in matrix A when are calculating the final result matrix. But Overall, it does not help improve run time algorithmatically!!
