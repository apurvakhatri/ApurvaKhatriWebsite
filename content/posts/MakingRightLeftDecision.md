# Making decision to go right or left with Binary Search. 


This post will be discussing **approaches** to certain problems which **strictly teaches binary search.**

I will not be presenting any solutions just discussing the approaches. 

# [Single Element in Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/description/)

## Description:

>You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

>Return the single element that appears only once.

> **Your solution must run in O(log n) time and O(1) space.**

Example 1:

Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2

Example 2:

Input: nums = [3,3,7,7,10,11,11]
Output: 10




### Now, Considering Example 1
> nums = [1,1,2,3,3,4,4,8,8]

> Middle = 4th Index i.e nums[4] = 3

### Task 1
Now, sitting at 4th index, I have to make a decision to either go right or left. 
First, I have to check whether the element I am at is the unique element or not. 

I do this by comparing it with its neighbours i.e (middle-1) and (middle+1).


> If middle element's value matches with (middle-1) that means I am at the **second occurrence** of 
> this element, and it is not unique

**OR**

> If middle element's value matches with (middle+1) that means I am at the **first occurrence** of 
> this element, and it is not unique.

```
If (neither of the above is true) then 
    the middle element is the unique element and I return its index. 

If(nums[3] == nums[4] || nums[4] == nums[5]) then

```

The first expression evaluates to true, **Where do I go now?** 

### Is the unique element on the right side or left side? 

Usually *this decision is made by comparison*, **but here with comparison I only got the info whether 
it is unique or not.** I did not get the info to move in which direction.

> **In Binary search comparison gives two things**
> 1. Evaluates to something (It is, or It is not)
> 2. Gives a direction to move (Right or left)

> It is given that the :
> 
> Array is sorted and there is **<u>one unique element</u>**. Every element occurs twice.
> 
> Now, **Considering** that the 
> 
> Array is sorted and there are **<u>no unique elements</u>**. Every element occurs twice. 

In that case the arrays would be like this
> nums = [1,1,2,2,3,3,4,4,8,8]

##### In this scenario every <u>first occurrence</u> of every unique element is at an <u>odd position</u> and every <u>second occurrence</u> of every unique element is at <u>even position</u>. 

> [If considering 0 based index then do +1]

##### Now if I remove xth element (x < nums.size()) the elements of the array before the xth position still follows the above principle, but the principle is disrupted for elements after that position.

> Since we are using 0 based index for computation we have to consider its index + 1 to know whether it is at odd position or even. 

> For ex: If I remove 4th index
> 
> nums = [1,1,2,2,3,4,4,8,8]
> 
> _Brackets denote position index_
>
> The elements before that [1(1),1(2),2(3)] 
> 
> The elements after that [3(4), 3(5), 4(6), 4(7), 8(8), 8(9)]

##### So this information will help me decide which direction to move in. 

>> If I am at the middle element,
and it is the <u>first occurrence</u> and the **position** *(Should be odd)* <u>is not odd</u>, 
which means
> 
>> **Disruption happened before middle element and therefore unique element lies to the left**.
>
>> Else move right, the unique element has not occurred yet.

>> If I am at the middle element, 
and it is the <u>second occurrence</u> and the **position** *(Should be even)* <u>is not even</u>,
which means 
> 
>> **Disruption happened before middle element and therefore unique element lies to the left**. 
> 
>> Else move right, the unique element has not occurred yet. 

```
Similarily for this example: 
nums = [1,1,2,3,3,4,4,8,8]

middle = 4th index
middle Element = nums[4] = 3, which is the second occurrence 
(4 + 1 = 5, Remember +1 for 0 based index) and is not at 
even position therefore disruption happened before this index. 
So we will move left 

Now
low = 0, high = 3
middle = 1
middle Element = nums[1] = 1, which is the second occurence 
and is at even position. 
We know disruption has not happened yet
So unique element is to the right 

Now 
low = 2, high = 3
middle = 3
middle Element = nums[3] = 3, which is the first occurence 
and is not at odd position therefore we move left.

Now 
low = 2, high =2 
low==high, So we are at the middle most element. 
This can even be confirmed by comparing with (middle-1)
if it is not out of array or with (middle+1)
if it is not out of array. 
But in worst case we will be able to compare with neither of the one.

```

#### The above applies only when question says that unique element is there]


If the question says there **might** be an unique element **and if it is there is only 1** 
then we have to compare for low==high also because there is a possibility for not existing
in which the answer would be -1;









