---
layout: default
title: "Crossword puzzle cropping"
description: Crossword puzzle cropping
date: 2024-03-28
math: true
---

# Cropping a crossword puzzle with computer vision

Say you want to extract a crossword puzzle out of the following image:

<img src="/assets/images/computer-vision/1-page.png" style="max-width:600px; width:100%;">

How would you do it?

Well, let's start by converting the image to grayscale:

```python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
```

<img src="/assets/images/computer-vision/2-grayscale.png" style="max-width:600px; width:100%;">

Let's crop out the header and footer to narrow down our region of interest. Why?
Well we're going to eventually run a contour-detection algorithm and we want to
filter away as much noise as possible:

```python
height, width = gray.shape

roi_y1, roi_y2 = int(height*0.30), int(height*0.85)
roi_x1, roi_x2 = int(width*0.20), int(width*0.85)

gray_roi = gray[roi_y1:roi_y2, roi_x1:roi_x2]
```

<img src="/assets/images/computer-vision/3-gray-roi.png" style="max-width:600px; width:100%;">

Great! Now let's binarize the image:

```python
binarized_roi = cv2.adaptiveThreshold(
    gray_roi, 255, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY_INV, 31, 10
)
```

<img src="/assets/images/computer-vision/4-binarized-roi.png" style="max-width:600px; width:100%;">

Okay! Now let's run a long thin horizontal kernel to filter away everything that
isn't a long thin horizontal line. Apply [erosion and
dilation](https://docs.opencv.org/3.4/db/df6/tutorial_erosion_dilatation.html)
to help fill in the gaps:

```python
scale = 15  # adjust per image size

horiz_kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (gray_roi.shape[1]//scale, 1))

horiz = cv2.dilate(cv2.erode(binarized_roi, horiz_kernel, 1), horiz_kernel, 1)
```

<img src="/assets/images/computer-vision/5-horiz-roi.png" style="max-width:600px; width:100%;">

Now do the same thing with a long thin vertical kernel, also applying erosion
and dilation:

```python
scale = 15  # adjust per image size

vert_kernel  = cv2.getStructuringElement(cv2.MORPH_RECT, (1, gray_roi.shape[0]//scale))

vert  = cv2.dilate(cv2.erode(binarized_roi, vert_kernel, 1),  vert_kernel, 1)
```

<img src="/assets/images/computer-vision/6-vert-binarized.png" style="max-width:600px; width:100%;">

Now let's add the horiztonal and vertical bitmaps back together:

```python
grid = cv2.addWeighted(horiz, 0.5, vert, 0.5, 0)
```

<img src="/assets/images/computer-vision/8-added-binarized.png" style="max-width:600px; width:100%;">

Now it's time for the magic - let's run a contour detection algorithm over the
bitmap. This is basically finding blobs with their perimeter:

```python
contours, _ = cv2.findContours(grid, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
```

<img src="/assets/images/computer-vision/9-contours.png" style="max-width:600px; width:100%;">

Looks like there are a few contours, including the crossword contour! We need
some way to rank them - let's do it by area and width/height ratio:

```python
candidates = []
for contour in contours:
    x, y, contour_width, contour_height = cv2.boundingRect(contour)
    contour_area = contour_width * contour_height
    width_height_contour_ratio = contour_width / float(contour_height)
    if contour_area > 15000 and 0.8 < width_height_contour_ratio < 1.2:  # large and roughly square
        candidates.append((contour_area, x, y, contour_width, contour_height))

top_candidate = candidates[0]
```

<img src="/assets/images/computer-vision/10-top-contour.png" style="max-width:600px; width:100%;">

Looks good! Finally, let's use the top candidate to crop the crossword from the
original image:

```python
_, x, y, contour_width, contour_height = top_candidate

crop = img_roi[y:y+contour_height, x:x+contour_width]
```

<img src="/assets/images/computer-vision/11-crop.png" style="max-width:600px; width:100%;">

And that's how you crop a crossword with computer vision!

## Credits

Thanks to ChatGPT for coming up with the pipeline and filling in the gaps.

Also thanks to OpenCV for their documentation on [Erosion and
Dilation](https://docs.opencv.org/3.4/db/df6/tutorial_erosion_dilatation.html) -
it's very well written!
