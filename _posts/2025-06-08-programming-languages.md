---
layout: post
title: "Programming Languages"
date: 2025-06-08 23:25:00 -0000
categories: programming
---

This is the sample Java code:

{% highlight java linenos mark_lines="1 2" %}

import java.util.*;
 
class Palindrome
{
   public static void main(String args[])
   {
      String original, reverse = "";
      Scanner in = new Scanner(System.in);
 
      System.out.println("Enter a string to check if it is a palindrome");
      original = in.nextLine();
 
      int length = original.length();
 
      for ( int i = length - 1; i >= 0; i-- )
         reverse = reverse + original.charAt(i);
 
      if (original.equals(reverse))
         System.out.println("Entered string is a palindrome.");
      else
         System.out.println("Entered string is not a palindrome.");
 
   }
}

{% endhighlight %}

This is sample C++ code: 

{% highlight cpp linenos %}
// C++ program to multiply two matrices

#include <bits/stdc++.h>
using namespace std;

// Edit MACROs here, according to your Matrix Dimensions for
// mat1[R1][C1] and mat2[R2][C2]
#define R1 2 // number of rows in Matrix-1
#define C1 2 // number of columns in Matrix-1
#define R2 2 // number of rows in Matrix-2
#define C2 3 // number of columns in Matrix-2

void mulMat(int mat1[][C1], int mat2[][C2])
{
    int rslt[R1][C2];

    cout << "Multiplication of given two matrices is:\n";

    for (int i = 0; i < R1; i++) {
        for (int j = 0; j < C2; j++) {
            rslt[i][j] = 0;

            for (int k = 0; k < R2; k++) {
                rslt[i][j] += mat1[i][k] * mat2[k][j];
            }

            cout << rslt[i][j] << "\t";
        }

        cout << endl;
    }
}

// Driver code
int main()
{
    // R1 = 4, C1 = 4 and R2 = 4, C2 = 4 (Update these
    // values in MACROs)
    int mat1[R1][C1] = { { 1, 1 }, { 2, 2 } };

    int mat2[R2][C2] = { { 1, 1, 1 }, { 2, 2, 2 } };

    if (C1 != R2) {
        cout << "The number of columns in Matrix-1 must "
                "be equal to the number of rows in "
                "Matrix-2"
             << endl;
        cout << "Please update MACROs according to your "
                "array dimension in #define section"
             << endl;

        exit(EXIT_FAILURE);
    }

    // Function call
    mulMat(mat1, mat2);

    return 0;
}
{% endhighlight %}

This is sample C code: 

{% highlight c linenos %}
static int get_random_numbers(u8 *buf, unsigned int len)
{
    struct crypto_rng *rng = NULL;
    char *drbg = "drbg_nopr_sha256"; /* Hash DRBG with SHA-256, no PR */
    int ret;

    if (!buf || !len) {
        pr_debug("No output buffer provided\n");
        return -EINVAL;
    }

    rng = crypto_alloc_rng(drbg, 0, 0);
    if (IS_ERR(rng)) {
        pr_debug("could not allocate RNG handle for %s\n", drbg);
        return PTR_ERR(rng);
    }

    ret = crypto_rng_get_bytes(rng, buf, len);
    if (ret < 0)
        pr_debug("generation of random numbers failed\n");
    else if (ret == 0)
        pr_debug("RNG returned no data");
    else
        pr_debug("RNG returned %d bytes of data\n", ret);

out:
    crypto_free_rng(rng);
    return ret;
}
{% endhighlight %}
