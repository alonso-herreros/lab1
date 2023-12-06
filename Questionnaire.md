## **Shared Access and Medium Network Architecure**

# **Laboratory Session 1: CRC Lab**

_Academic year 2023-2024_  
_Telematics Engineering Department - Universidad Carlos III de Madrid_

---

## Question 1
What is the minimum Hamming Distance $D$ of the codewords set?

> 3 ($\checkmark$)

## Question 2
What is the maximum number of bit errors that can be **detected**?

> 2 ($\checkmark$)

## Question 3
What is the maximum number of bit errors that can be **corrected**?

> 1 ($\checkmark$)

## Question 4
Paste here the code of the `hamming_distance` function

> ```python
> def hamming_distance(a,b):
>     if len(a)!=len(b): raise ValueError('The two codewords must have the same length')
>     #sum of: true for every bit that's different, false otherwise
>     return sum([a[i] != b[i] for i in range(len(a))])
> ```

## Question 5
Paste here the code of the `check_and_correct` function

> ```python
> #making use of functions and values obtained above
> def check_and_correct(w):
>     for codeword in codewords:
>         d = hamming_distance(w,codeword)
>         if d<=(best_D-1)//2: return d>0, codeword
>     return True, None
> ```

## Question 6
Paste here the code of the `check_and_correct_rectangular` function

> ```python
> def check_and_correct_rectangular(w):
>     a = w[:8].reshape(2,4)
>     row_p, col_p = w[8:10], w[10:]
>     row_check, col_check = np.mod(np.sum(a,axis=1),2), np.mod(np.sum(a,axis=0),2)
>     row_wrongs = [i for i in range(len(row_p)) if row_p[i]!=row_check[i]]
>     col_wrongs = [i for i in range(len(col_p)) if col_p[i]!=col_check[i]]
>     if len(row_wrongs)==0 and len(col_wrongs)==0: # No error
>         return False, None
>     elif len(row_wrongs)==1 and len(col_wrongs)==1: # Assume single-bit error in the data
>         a[row_wrongs[0], col_wrongs[0]] = 1-a[row_wrongs[0], col_wrongs[0]]
>     elif len(row_wrongs) + len(col_wrongs)==1: # Assume single-bit error in the parity bit
>         pass
>     else:
>         return True, None
>     return True, a.flatten()
> ```

## Question 7
Why is this method more efficient than the one in Step 0?

> Because we are using the same number of redundant bits, 5, to encode up to 8 bits of data instead
> of 3. However, if the errors occur in specific bits, they become harder to detect.

## Question 8
What is the maximum number of bit errors that can be **corrected**?

> We can correct up to 1 bit errors

## Question 9
What is the maximum number of bit errors that can be **detected**?

> We can detect up to 2 bit errors, unless they occur in one row and one column parity bits, in
> which case, the usual assumption is that the corresponding data bit was the one that was switched.

## Question 10
Introduce a bit error (bit flip) on bits 1,5,6,7, and 10 (from the left). Is the error detected by
CRC? What is the remainder?

> The error is detected.
> If no zero-padding is done (using the given function), the remainder is 13 (01101)
> If the data is padded with m-1 zeroes, the remainder is 2 (00010)

## Question 11
What codeword is finally sent?

> The final word without error is 1010011010011100
> The final word with induced error is 0010100011011100
