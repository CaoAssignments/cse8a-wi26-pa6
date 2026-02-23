# CSE 8A PA6 Lab

February 24, 2026

---

## Introduction

Welcome to your PA6 Lab!

Today you will practice pixel index calculations for rectangular regions in an image. This is super helpful for completing PA6. We will also cover common errors caused by `range(...)` and how to avoid them. Finally you will implement a `corners` function that computes all four corners of a region.

---

## Learning Goals
By the end of this lab, you will be able to:

- Calculate pixel indices of corners in a rectangular region
- Avoid off-by-one mistakes when using `range(a, b)`
- Calculate the width and height of a region created by nested loops
---

## Part 1: Calculating Indices in a Region

When manipulating pixels in an image, we often work with a rectangular region. To specify a region, we need to know the coordinates of its corners, especially the upper-left corner, which is often the starting point for loops. The upper-left corner is given by `img[row_index][col_index]`, and the region has a certain height of `region_height` and a width of `region_width`.


To visualize this, imagine an image as a 2D grid of pixels. If we want to define a specific rectangular region within that image, we need to know the exact coordinates of its four corners. Python uses **0-based indexing**, meaning both the row index and column index start at 0. Because of this, a sequence of a certain `length` always ends at index `length - 1`. Therefore, when calculating the end of a region, the last row or column is always the starting index plus the length, **minus 1**.

Here is how the coordinates map to the four corners of a region:

```text
                                col_index                       col_index + region_width - 1
                                      |                                            |
                                      v                                            v
                    row_index ------> +--------------------------------------------+
                                      | Top-Left                       Top-Right   |
                                      |                                            |
                                      |                                            |
                                      |                  REGION                    |
                                      |                                            |
                                      |                                            |
                                      | Bottom-Left                 Bottom-Right   |
row_index + region_height - 1 ------> +--------------------------------------------+
```

### A More Concrete Example
Let's look at a 6x6 image where we want to select a 3x4 region and start at `img[1][2]` (that means `row_index = 1`, `col_index = 2`, `region_height = 3`, and `region_width = 4`):

```text
col:        0   1   2   3   4   5
          +---+---+---+---+---+---+
    row 0 |   |   |   |   |   |   |
          +---+---+===============+
    row 1 |   |   ||TL|   |   |TR|| <-- Top edge (row_index = 1)
          +---+---+|--+---+---+--||
    row 2 |   |   ||  |   |   |  ||
          +---+---+|--+---+---+--||
    row 3 |   |   ||BL|   |   |BR|| <-- Bottom edge (1 + 3 - 1 = 3)
          +---+---+===============+
    row 4 |   |   |   |   |   |   |
          +---+---+---+---+---+---+
    row 5 |   |   |   |   |   |   |
          +---+---+---+---+---+---+
                    ^           ^
                    |           |
     Left edge -----+           +----- Right edge
     (col_index = 2)                   (2 + 4 - 1 = 5)
```

In this image example, the four corners of the region are:
- Top-left (TL): `img[1][2]` (This is `img[row_index][col_index]`)
- Top-right (TR): `img[1][5]` (This is `img[row_index][col_index + region_width - 1]`)
- Bottom-left (BL): `img[3][2]` (This is `img[row_index + region_height - 1][col_index]`)
- Bottom-right (BR): `img[3][5]` (This is `img[row_index + region_height - 1][col_index + region_width - 1]`)

### Off-by-One Errors
A common mistake is to forget to subtract 1 when calculating the last row or column of a region. In Python, `range(a, b)` includes `a` but does not include `b`, so the last index is always `b - 1`. Similarly, when calculating corners, we need to subtract 1 from the width and height to get the correct indices.

Off-by-one errors can lead to trying to access pixels that are outside the bounds of the image, which will cause an `IndexError`. Always double-check your calculations to ensure you are accessing valid indices.


Off-by-one errors can also lead to incorrect region sizes. For example, if you forget to subtract 1, you might end up with a region that is one pixel larger than intended, which can cause unexpected behavior in your image processing tasks. For example, in the following code:

```python
# We want to process a region starting at column 2 and ending at column 5 (inclusive).
# The pixels included are at indices 2, 3, 4, and 5.
start_col = 2
end_col = 5

# ❌ INCORRECT width calculation:
wrong_width = end_col - start_col
print(wrong_width) # Outputs 3. (We missed a pixel! The region is too small.)

# ✅ CORRECT width calculation:
correct_width = end_col - start_col + 1
print(correct_width) # Outputs 4. (Columns 2, 3, 4, 5 equals 4 total pixels.)
```


### Examples

Let's work out some examples to solidify our understanding. Try to answer these before looking at the solution!

**Question 1: The Standard Region**
You want to define a region starting at `row_index = 2` and `col_index = 3`. The region has a `region_height = 4` and `region_width = 5`.
What are the coordinates for the **Bottom-Right (BR)** corner?
- A) `img[6][8]`
- B) `img[5][7]`
- C) `img[5][8]`
- D) `img[6][7]`

<details>
<summary><strong>Click here for the answer</strong></summary>
<strong>Answer: B) <code>img[5][7]</code></strong><br>
<em>Explanation:</em> The bottom-right row is <code>row_index + region_height - 1</code> (2 + 4 - 1 = 5). The bottom-right column is <code>col_index + region_width - 1</code> (3 + 5 - 1 = 7). Option A is the classic off-by-one error where you forget to subtract 1!
</details>

<br>

**Question 2: The 1D Strip**
You are cropping a horizontal strip of the image. The starting pixel is `row_index = 0` and `col_index = 0`. The region dimensions are `region_height = 1` and `region_width = 10`.
What are the coordinates for the **Bottom-Left (BL)** and **Top-Right (TR)** corners?
- A) BL: `img[1][0]`, TR: `img[0][10]`
- B) BL: `img[0][0]`, TR: `img[0][10]`
- C) BL: `img[0][0]`, TR: `img[0][9]`
- D) BL: `img[1][1]`, TR: `img[1][10]`

<details>
<summary><strong>Click here for the answer</strong></summary>
<strong>Answer: C) BL: <code>img[0][0]</code>, TR: <code>img[0][9]</code></strong><br>
<em>Explanation:</em> Because the height is 1, the top and bottom rows are the exact same! <code>0 + 1 - 1 = 0</code>. For the Top-Right column, <code>0 + 10 - 1 = 9</code>.
</details>

<br>

**Question 3: Spot the Bug**
A student writes the following logic to find the four corners of a 5x5 region starting at `row_index = 4` and `col_index = 4`:
* Top-Left: `img[4][4]`
* Top-Right: `img[4][9]`
* Bottom-Left: `img[9][4]`
* Bottom-Right: `img[9][9]`

What mistake did the student make?
- A) They swapped the rows and columns.
- B) They forgot that Python uses 1-based indexing.
- C) They added the width/height to the starting index but forgot to subtract 1.
- D) They didn't make a mistake; these coordinates are correct.

<details>
<summary><strong>Click here for the answer</strong></summary>
<strong>Answer: C) They added the width/height to the starting index but forgot to subtract 1.</strong><br>
<em>Explanation:</em> If you start at index 4 and want 5 total pixels, your indices are 4, 5, 6, 7, and 8. The correct corners should end at index 8 (<code>4 + 5 - 1 = 8</code>). By using 9, they accidentally created a 6x6 region!
</details>



## Part 2: Implement Corner Calculations
To practice calculating the corner indices, let's implement a function `corners` that takes the `row_index`, `col_index`, `region_height`, and `region_width` as parameters and returns the indices of all four corners of the region as a tuple in the format `(upper_left, upper_right, bottom_left, bottom_right)`. Each corner should be represented as a tuple of `(row_index, col_index)`.

```python
def corners(row_index, col_index, region_height, region_width):
    """Return (upper_left, upper_right, bottom_left, bottom_right)."""
    # Calculate upper-left corner

    # Calculate upper-right corner

    # Calculate bottom-left corner

    # Calculate bottom-right corner

    # Return the corners as a tuple
```

Examples that you can test with:

```python
print(corners(1, 2, 3, 4))
# Expected output: ((1, 2), (1, 5), (3, 2), (3, 5))

print(corners(0, 0, 1, 1))
# Expected output: ((0, 0), (0, 0), (0, 0), (0, 0))
# This is a 1x1 region, so all corners are the same.

print(corners(4, 7, 3, 5))
# Expected output: ((4, 7), (4, 11), (6, 7), (6, 11))
```

---

## Part 3: Lab Quiz (~15 minutes)
Make sure to review the lab activity today! The lab quiz will test material based on what you learned in this lab.