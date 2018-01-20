---
layout: "post"
title: "Detailed explanation of a smart solution to an algo problem beating 99.9% submission"
comments: true
image: attachments_article_algorithm_col_slide_lamparas-colgantes-algorithm-slide-03.jpg
---

This post is about a coding problem and why the solution I pasted down below is smart.
<br/>
<br/>
Problem:<br/>

```

Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |

```

If it is of your interest, I would recommend you take a few minutes to think about how you would approach this problem! <br/>
<br/>

The main focus of this post is to 1)explain in detail why the provided solution is smart and 2)make some improvements/tweaks in the code of the smart solution to show you which part is really essential, 3) also i will briefly mention why <a href="http://www.cs.cmu.edu/~scandal/cacm/node9.html">Sparse Matrix Manipulation</a> can help make some improvements on top of the smart solution.<br/>
<br/>
a) Originally, the normal way to calculate the multiplication of two metrics A, and B is as follow:
We take the the all values from the first line of A, and all values from the first column of B, and multiply the corresponding values and sum them up, the final sum is the value for the location of first column and first row in final result matrix. Similarly, the value at [ i ][ j ] of result matrix C, which is C[ i ][ j ] is calculated as:<br/>
<br/>
C[ i ][ j ] = A[ i ][0]*B[0][j] + A[i][1]*B[1][j] + A[i][2]*B[2][j] + ... A[i][K]*B[K][j]<br/>
(which is the sum of each multiplication of corresponding K values from row i of A and K values from column j of B)<br/>
`The Key is: if we calculate it this way, we finishing calculating the final value for the result matrix at once` <br/>
<br/>
Then a brute force solution is as follow:<br/>

{% highlight java %}
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
{% endhighlight %}

b) <Strong>The smart solution</Strong>: the key part of smart solution is that: it does not calculate the final result at once, and it takes each value from A, and calculate and partial sum and accumulate it into the final spot:<br/>
For example, for each value A[i][k], if it is not zero, it will be used at most nB times ( n is B[0].length ), which can be illustrated as follow:
Generally for the following equation:<br/>
<br/>
C[i][j] = A[i][0]*B[0][j] + A[i][1]*B[1][j] + A[i][2]*B[2][j] + ... A[i][k]*B[k][j] .... A[i][K]*B[K][j]  
<br/>
j can be from 0 to nB, if we write all of them down, it will like following:
<br/>
<br/>
For i from 0 to nB:<br/>
C[ i ][ 0 ]=A[ i ][0]*B[0][0] + A[i][1]*B[1][0] + A[i][2]*B[2][0] + ... A[i][k]B[k][0] .... A[i][K]*B[K][0] <br/>
C[ i ][ 1 ]=A[ i ][0]*B[0][1] + A[i][1]*B[1][1] + A[i][2]*B[2][1] + ... A[i][k]B[k][0] .... A[i][K]*B[K][1] <br/>
... <br/>
C[ i ][ nB ]=A[ i ][0]*B[0][nB] + A[i][1]*B[1][nB] + A[i][2]*B[2][nB] + ... A[i][k]B[k][nB] .... A[i][K]*B[K][nB] <br/>
<br/>
As you can see from above: for the same value A[i][k] from the first matrix, it will be used at most nB times if A[i][k] is not zero. And the smart solution is taking advantage of that!!!, the smart solution can be described as: <br/>
<br/>
For each value A[i][k] in matrix A, if it is not zero, we calculate A[i][k] * B[k][j] and accumulate it into C[ i ][ j ]  (`Key part: the C[ i ][ j ] by now is not the final value in the result matrix !! Remember, in the brute force solution, the final value of C[i][j], takes sum of all multiplication values of K corresponding values from A and B? here C[ i ][ j ] is only sum of some multiplication values, NOT ALL until the program is done`)
<br/>
<br/>
<Strong>BY NOW, it is very clear that, if the value A[ i ][ k ] from matrix is zero, we skip a For-loop- calculation, which is a loop iterating nB times, and this is the key part of why the smart solution is smart!!! </Strong> <br/>
<br/>
The smart solution code is as follow:<br/>

{% highlight java %}
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
{% endhighlight %}

(Credit:@yavinci; I am having a different version of the solution, so I am directly referencing the original version as a reference to demonstrate how mine is different). <br/>
<br/>

Based on the discussion above, the inner checking (`if (B[k][j] != 0)`) is actually not necessary, because whether or not we have that check, we still iterate nB times, ( since the operation `C[i][j] += A[i][k] * B[k][j];`  inside the if-check is O(1) time) <br/>
<br/>
So the smart solution can also be written as follow by removing the check ( which is my version ): <br/>

{% highlight java %}
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
{% endhighlight %}

<br/>
c) "Sparse matrix manipultion" helps, if we compress the first sparse matrix into rows of lists( in each row list, it contains ( value, index ) pair ), we actually don't need to go over all values in a row in matrix A when are calculating the final result matrix. But Overall, it does not help improve run time algorithmatically!!

<br/>
<br/>

References:<br/>
1. Image credit: attachments_article_algorithm_col_slide_lamparas-colgantes-algorithm-slide-03.jpg 

{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}
