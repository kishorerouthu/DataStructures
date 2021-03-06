Maximum sum sub-array(MSS) problem is the task of finding the contiguous sub-array within a one-dimensional array of
n numbers which has largest sum.

EXAMPLE :
    For sequence of values -2, 1, -3, 4, -1, 2, 1, -5, 4 ;  the contiguous sub-array with largest sum is
    4, -1, 2, 1 with sum 6;


METHOD 1:  Brute Force
           1. Sizes of all possible sub-arrays are from 1...n;
           2. For each size Find the max size of sum all possible sub-arrays of that size
           3. Find the maximum of all sizes of sub-arrays

           ALGORITHM :
               fun(arr[], n) {
                    max_sum = -INF
                    for (sub_array_size -> 1 to n) {
                        for (startIndex -> 0 to n-1) {
                            if(startIndex + sub_array_size > n)
                                break;
                            sum = 0;
                            for (i -> startIndex to startIndex + sub_array_size)
                                sum = sum + arr[i];
                            max_sum = max(max_sum, sum);
                        }
                    }
                    return max_sum
               }

               Time complexity : O(n^3)


           OPTIMISED ALGORITHM :
                In this algorithm we take the already calculated sum previous (less size) sub-array to calculate next
                sub-array at each index.

                Example :

                For index 0 all possible sub-arrays
                Size                                              Sum
                1        -2                              =              -2
                2        -2, 1                           =  -2 + 1      -1
                3        -2, 1, -3                       =  -1 + -3     -4
                4        -2, 1, -3, 4                    =  -4 + 4       0
                5        -2, 1, -3, 4, -1                =  0 + -1      -1
                6        -2, 1, -3, 4, -1, 2             =  -1 + 2       1
                7        -2, 1, -3, 4, -1, 2, 1          =  1 + 1        2
                8        -2, 1, -3, 4, -1, 2, 1, -5      =  2 + -5      -3
                9        -2, 1, -3, 4, -1, 2, 1, -5, 4   =  -3 + 4       1


                For index 1 all possible sub-arrays
                Size                                           Sum
                1        1                           =              1
                2        1, -3                       =  1 + -3     -2
                3        1, -3, 4                    =  -2 + 4      2
                4        1, -3, 4, -1                =  2 + -1      1
                5        1, -3, 4, -1, 2             =  1 + 2       3
                6        1, -3, 4, -1, 2, 1          =  3 + 1       4
                7        1, -3, 4, -1, 2, 1, -5      =  4 + -5     -1
                8        1, -3, 4, -1, 2, 1, -5, 4   =  -1 + 4      3


                For index 2 all possible sub-arrays
                Size                                           Sum
                1        -3                       =             -3
                2        -3, 4                    =  -3 + 4      1
                3        -3, 4, -1                =  1 + -1      0
                4        -3, 4, -1, 2             =  0 + 2       2
                5        -3, 4, -1, 2, 1          =  2 + 1       3
                6        -3, 4, -1, 2, 1, -5      =  3 + -5     -2
                7        -3, 4, -1, 2, 1, -5, 4   =  -2 + 4      2

                For index 3 all possible sub-arrays
                Size                                        Sum
                1        4                    =              4
                2        4, -1                =  4 + -1      3
                3        4, -1, 2             =  3 + 2       5
                4        4, -1, 2, 1          =  5 + 1       6
                5        4, -1, 2, 1, -5      =  6 + -5      1
                6        4, -1, 2, 1, -5, 4   =  1 + 4       5

                etc....
                we found max 6 at 4, -1, 2, 1

                fun(arr[], n) {
                        max_sum = -INF
                        for (startIndex -> 0 to n-1) {
                            sum = 0;
                            for(sub_array_size -> 1 to n) {
                                if(startIndex + sub_array_size > n)
                                    break;
                                sum = sum + arr[startIndex + sub_array_size-1];
                                max_sum = max(sum, max_sum);
                            }
                        }
                        return max_sum;
                }

                Time complexity : O(n^2)

METHOD 2 : Divide and Conquer
           Divide the array in to 2 half's and find the maximum sum of each part recursively and find the crossing (
           left and right) sum as well.

           ALGORITHM :
                        maximum_sum_sub_array(arr[], n) {
                               if (n == 1)
                                    return arr[0];
                               m = n / 2;

                               LeftMSS = maximum_sum_sub_array(arr, m);
                               RightMSS = maximum_sum_sub_array(arr, n-m);

                               leftSum = -INF;
                               rightSum = -INF;
                               sum = 0;

                               for(i-> m to n) {
                                    sum += arr[i];
                                    rightSum = max (rightSum, sum);
                               }

                               sum = 0;
                               for(i->m-1 to 0) {
                                    sum += arr[i];
                                    leftSum = max (leftSum, sum);
                               }
                               ans = max(LeftMSS, rightMSS);
                               return max(ans, leftSum + rightSum);
                        }

               Evaluation :
                                        7
                                [-2, -3, 4, -1, -2, 1, 5, -3]
                            4                               6
                         [-2,-3,4,-1]                 [-2, 1, 5, -1]
                         -2         4                   1          5
                     [-2,-3]     [4,-1]             [-2,1]      [5,-1]
                  -2        -3  4      -1         -2       1    5     -1
                    [-2] [-3]   [4]  [-1]         [-2]  [1]    [5]  [-1]



METHOD 3 : Kadane's Algorithm
           Simple idea of the Kadane's algorithm is to look for all positive contiguous segments of the array.
           And keep track of maximum sum contiguous segment among all positive segments. Each time we get a positive
           sum compare it with max so far and update max so far if it is greater than max_so_far.

           MSS(arr[], n) {
                sum = 0;
                ans = 0;
                for (i -> 0 to n) {
                    if (sum + arr[i] > 0) {
                        sum += arr[i];
                        mss = max (sum, ans);
                    } else {
                        sum = 0;
                    }
                }
                return ans;
           }

           Note : Algorithm doesn't work for all negative numbers. It simply returns 0 if all numbers are negative.
                  For handling this we can add an extra phase before actual implementation.
                  The phase will look if all numbers are negative, if they are it will return maximum of them (or
                  smallest in terms of absolute value). There may be other ways to handle it though.

           Following algorithm works even for all negative numbers;

           MSS(arr[], n) {
                sum = 0;
                ans = -INF;
                neg = -INF;
                for (i -> 0 to n) {
                    if(sum + arr[i] > 0){
                        sum += arr[i];
                        ans = max(ans, sum);
                    } else {
                        sum = 0;
                    }

                    if(arr[i] > 0)
                        neg = max(arr[i], neg);
                }
                return max(ans, neg);
           }
