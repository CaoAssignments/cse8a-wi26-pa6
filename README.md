# CSE 8A Winter 2026 PA6

**Due Date: Sunday, March 1, 2026, 11:59 PST**

## Provided Files
None

## File(s) to Submit
- `flip.py`

(Details on how to submit your files can be found below)

---

## Part 0. Environment Setup

Before starting this assignment, please make sure you have successfully set up the CSE8A Image library and its dependencies. If you have no issue with environment setup from PA5, you should be good to go for this PA as well.

If you have not set up the environment yet, please refer to the [instructions](https://github.com/CaoAssignments/cse8a-wi26-pa5/blob/main/README.md#part-0-environment-setup) in PA5.


## Part 1. Implementation (100 points)

In this programming assignment, you will implement image flipping functions that flip a rectangular region of an image vertically from top to bottom.

To start off, you will implement a helper function `check_and_clip` that checks the validity of the input parameters and clips the region to fit within the image boundaries if necessary.

Then you will implement the main function `flip_image` that performs the vertical flip on the specified region using the helper function.

Both functions will have their own test cases on Gradescope, so make sure to implement both functions correctly.

### Function Overview
1. **`check_and_clip(image, col_index, row_index, region_height, region_width)`**
   Checks the validity of the input parameters and clips the region to fit within the image boundaries if necessary. Returns a tuple of the form `(is_valid, clipped_col_index, clipped_row_index, clipped_region_height, clipped_region_width)`. More details in Part 2 below.

2. **`flip_image(image, col_index, row_index, region_height, region_width)`**
   Flips a selected rectangular region of the image vertically from top to bottom. The region is defined by the starting pixel at `(col_index, row_index)` and the specified `region_height` and `region_width`. The function should use the `check_and_clip` helper function to validate and adjust the region as needed. The flip should be performed in-place. The function returns a boolean indicating whether the flip was successful (i.e., the input parameters were valid and the flip was performed). More details in Part 3 below.

## Part 2: Function 1: `check_and_clip` (40 points)

### Part 2.1 Task Description
Write a function that checks the validity of the input parameters and clips the region to fit within the image boundaries if necessary.

The input parameters are considered valid if:
- All parameters are integers (In this lab, all input parameters will be integers, so no need to explicitly check this) 
- The starting pixel is within the bounds of the image, i.e., `0 <= col_index < image width` and `0 <= row_index < image height`
- The `region_height` and `region_width` are greater than 0

If the parameters are valid, clipping should be performed as follows:
- If the region extends beyond the right edge of the image, adjust `region_width` to fit within the image, name the new width `clipped_region_width`. I.e. if `col_index + region_width > image width`, set `region_width` to `image width - col_index`.
- If the region extends beyond the bottom edge of the image, adjust `region_height` to fit within the image, name the new height `clipped_region_height`. I.e. if `row_index + region_height > image height`, set `region_height` to `image height - row_index`.

Finally the function should return a tuple of the form `(is_valid, col_index, row_index, clipped_region_height, clipped_region_width)`, where `is_valid` is a boolean indicating whether the input parameters are valid, and the other values represent the potentially clipped region parameters. For invalid cases, the clipped parameters should all be set to `None`.

### Part 2.2 Implementation Details

```python
# Make sure to import the CSE8AImage Library at the start of your file
from CSE8AImage import *

def check_and_clip(image, col_index, row_index, region_height, region_width):
    # check if parameters are valid, return (False, None, None, None, None) if not valid

    # if valid, clip the region to fit within the image boundaries if necessary

    # return valid and clipped parameters as a tuple
```

### Part 2.3 Examples

```python
# For these examples, we will be using the following made-up 5x5 image represented as a 2D list of RGB tuples:
img = [
    [(0, 0, 0), (0, 1, 0), (0, 2, 0), (0, 3, 0), (0, 4, 0)],
    [(1, 0, 0), (1, 1, 0), (1, 2, 0), (1, 3, 0), (1, 4, 0)],
    [(2, 0, 0), (2, 1, 0), (2, 2, 0), (2, 3, 0), (2, 4, 0)],
    [(3, 0, 0), (3, 1, 0), (3, 2, 0), (3, 3, 0), (3, 4, 0)],
    [(4, 0, 0), (4, 1, 0), (4, 2, 0), (4, 3, 0), (4, 4, 0)]
]
# Example 1: Valid region, completely inside the image. No clipping needed.
check_and_clip(img, 1, 1, 2, 3)
# Returns: (True, 1, 1, 2, 3)

# Example 2: Valid starting pixel, but region goes past the right edge.
# The region width is clipped to fit: image width (5) - col_index (2) = 3
check_and_clip(img, 2, 1, 2, 5)
# Returns: (True, 2, 1, 2, 3)

# Example 3: Valid starting pixel, but region goes past the bottom edge.
# The region height is clipped to fit: image height (5) - row_index (3) = 2
check_and_clip(img, 1, 3, 4, 2)
# Returns: (True, 1, 3, 2, 2)

# Example 4: Valid starting pixel, but region goes past both right and bottom edges.
# Both width and height are clipped.
check_and_clip(img, 3, 3, 10, 10)
# Returns: (True, 3, 3, 2, 2)

# Example 5: Invalid starting pixel (col_index is out of bounds).
check_and_clip(img, 5, 0, 2, 2)
# Returns: (False, None, None, None, None)

# Example 6: Invalid starting pixel (row_index is negative).
check_and_clip(img, 0, -1, 2, 2)
# Returns: (False, None, None, None, None)

# Example 7: Invalid region dimension (region_width is 0).
check_and_clip(img, 0, 0, 2, 0)
# Returns: (False, None, None, None, None)
```

### Part 2.4 Testing

To test your function, try using the following code:

```python
# Create a sample image (5x5) for testing
img = [
    [(0, 0, 0), (0, 1, 0), (0, 2, 0), (0, 3, 0), (0, 4, 0)],
    [(1, 0, 0), (1, 1, 0), (1, 2, 0), (1, 3, 0), (1, 4, 0)],
    [(2, 0, 0), (2, 1, 0), (2, 2, 0), (2, 3, 0), (2, 4, 0)],
    [(3, 0, 0), (3, 1, 0), (3, 2, 0), (3, 3, 0), (3, 4, 0)],
    [(4, 0, 0), (4, 1, 0), (4, 2, 0), (4, 3, 0), (4, 4, 0)]
]
# Test cases
print(check_and_clip(img, 1, 1, 2, 3)) # Expected: (True, 1, 1, 2, 3)
print(check_and_clip(img, 3, 3, 10, 10)) # Expected: (True, 3, 3, 2, 2)
print(check_and_clip(img, 5, 0, 2, 2)) # Expected: (False, None, None, None, None)
```

### Part 2.5 Hidden Testing
This PA contains 3 hidden tests, they include:

- `check_and_clip`: An edge case where the region is exactly the whole image boundary
- `flip_image`: A case where the flipped region is 1x1, so the function call should not alter anything 
- `flip_image`: A case where the entire image is flipped, checking if the final result is correct
## Part 3: Function 2: `flip_image` (60 points)

### Part 3.1 Task Description
Write a function that flips a selected rectangular region of the image vertically from top to bottom. The region is defined by the starting pixel at `(col_index, row_index)` and the specified `region_height` and `region_width`. The function should use the `check_and_clip` helper function to validate and adjust the region as needed. The flip should be performed in-place. The function returns a boolean indicating whether the flip was successful (i.e., the input parameters were valid and the flip was performed).

### Part 3.2 Implementation Details

```python
# Make sure to import the CSE8AImage Library at the start of your file
from CSE8AImage import *

def flip_image(image, col_index, row_index, region_height, region_width):
    # Use check_and_clip to validate and adjust the region parameters

    # If the parameters are not valid, return False

    # If valid, perform the vertical flip on the specified region in-place

    # Return True to indicate the flip was successful
```

### Part 3.3 Examples

```python
# Using the same sample image as before:
img = [
    [(0, 0, 0), (0, 1, 0), (0, 2, 0), (0, 3, 0), (0, 4, 0)],
    [(1, 0, 0), (1, 1, 0), (1, 2, 0), (1, 3, 0), (1, 4, 0)],
    [(2, 0, 0), (2, 1, 0), (2, 2, 0), (2, 3, 0), (2, 4, 0)],
    [(3, 0, 0), (3, 1, 0), (3, 2, 0), (3, 3, 0), (3, 4, 0)],
    [(4, 0, 0), (4, 1, 0), (4, 2, 0), (4, 3, 0), (4, 4, 0)]
]

# Note: For each example below, assume we start with the original 'img' defined above.

# Example 1: Valid region, no clipping needed.
# We want to flip a 3x3 region starting at row 1, col 1.
# This affects rows 1, 2, 3 and cols 1, 2, 3.
result1 = flip_image(img, 1, 1, 3, 3)
# result1 is True
# The region is flipped vertically. Row 1 swaps with Row 3. Row 2 stays the same.
# img becomes:
# [
#    [(0, 0, 0), (0, 1, 0), (0, 2, 0), (0, 3, 0), (0, 4, 0)],
#    [(1, 0, 0), (3, 1, 0), (3, 2, 0), (3, 3, 0), (1, 4, 0)], # Swapped with row 3
#    [(2, 0, 0), (2, 1, 0), (2, 2, 0), (2, 3, 0), (2, 4, 0)], # Unchanged (middle row)
#    [(3, 0, 0), (1, 1, 0), (1, 2, 0), (1, 3, 0), (3, 4, 0)], # Swapped with row 1
#    [(4, 0, 0), (4, 1, 0), (4, 2, 0), (4, 3, 0), (4, 4, 0)]
# ]


# Example 2: Valid starting pixel, but region requires bottom edge clipping.
# We want to flip a 4x2 region starting at row 3, col 2.
# The height (4) goes out of bounds, so check_and_clip reduces the height to 2 (rows 3 and 4).
result2 = flip_image(img, 2, 3, 4, 2)
# result2 is True
# The clipped 2x2 region is flipped vertically. Row 3 swaps with Row 4 for cols 2 and 3.
# img becomes:
# [
#    [(0, 0, 0), (0, 1, 0), (0, 2, 0), (0, 3, 0), (0, 4, 0)],
#    [(1, 0, 0), (1, 1, 0), (1, 2, 0), (1, 3, 0), (1, 4, 0)],
#    [(2, 0, 0), (2, 1, 0), (2, 2, 0), (2, 3, 0), (2, 4, 0)],
#    [(3, 0, 0), (3, 1, 0), (4, 2, 0), (4, 3, 0), (3, 4, 0)], # Swapped with row 4
#    [(4, 0, 0), (4, 1, 0), (3, 2, 0), (3, 3, 0), (4, 4, 0)]  # Swapped with row 3
# ]


# Example 3: Invalid parameters.
# The col_index is -1, which is invalid.
result3 = flip_image(img, -1, 0, 2, 2)
# result3 is False
# The image remains completely unchanged.
```

**Testing with provided images**: we provide sample images including `beach.jpg` and `autumn_leaves.jpg`. If you function works correctly, you should be able to see the flipped regions. For example, if you flip a 200x200 region starting at (100, 100) in `beach.jpg`, you should see the top part of that region swapped with the bottom part.


![beach](./beach.jpg)
![beach_flipped](./beach_flipped.jpg)

## Part 4: Submission

When you are ready, submit your code to Gradescope.

**You must submit this file:**
- `flip.py`

The file name must match exactly to pass the autograder.

### How to Submit to Gradescope

1. Go to Gradescope and find the PA6 assignment
2. Upload `flip.py`
2. Click "Submit"
3. Wait for the autograder to run

### Understanding Your Results

This PA uses **20 test cases total**:
- 8 tests for `check_and_clip` (40 points)
- 12 tests for `flip_image` (60 points)

If you fail a test case, Gradescope feedback may show:
- The test name (which tells you what it's testing)
- The input parameters
- The expected output
- Your actual output

You can submit multiple times. Use the feedback to debug and improve your code.

When you pass all public test cases, you should receive most of the points. Hidden tests help us check additional edge cases and discourage hard-coding.


### Getting Help
- **Office hours**: Attend tutor or TA office hours
- **Piazza**: Post questions on Piazza (don't share your code publicly!)
- **Start Early**: Give yourself plenty of time to work on the assignment and debug any issues that arise.
