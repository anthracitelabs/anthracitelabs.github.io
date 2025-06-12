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
#define R1 2 // number of rows in Matrix-1

void mulMat(int mat1[][C1], int mat2[][C2])
{
    int rslt[R1][C2];

    std::cout << "Multiplication of given two matrices is:\n";

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

int main()
{
    if (C1 != R2) {
        mulMat(mat1, mat2);    
        exit(EXIT_FAILURE);
    }
    return 0;
}
{% endhighlight %}

This is sample C code: 

{% highlight c linenos %}
#include <stdio.h>
// Edit MACROs here, according to your Matrix Dimensions for
#define R1 2 // number of rows in Matrix-1

void mulMat(int** mat1, int** mat2)
{
    int rslt[R1][C2];

    printf("Multiplication of given two matrices is:\n");

    for (int i = 0; i < R1; i++) {
        for (int j = 0; j < C2; j++) {
            rslt[i][j] = 0;

            for (int k = 0; k < R2; k++) {
                rslt[i][j] += mat1[i][k] * mat2[k][j];
            }

            printf("%d\t", rslt[i][j]);
        }
    }
}
{% endhighlight %}
