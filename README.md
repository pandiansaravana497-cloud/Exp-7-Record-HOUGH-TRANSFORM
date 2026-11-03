#  Lane Detection

##  Aim

To implement a basic lane detection pipeline using OpenCV by completing missing code segments at specified locations.

---

## Learning Objective

* Understand each stage of image processing
* Learn how to build a complete computer vision pipeline
* Practice writing code in guided sections

---

##  Software Used

* Anaconda – Python 3.7
* Jupyter Notebook / VS Code
* OpenCV (cv2)
* NumPy
* Matplotlib

---

##  Algorithm & Explanation

---

###  Step 1: Import Libraries

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

---

###  Step 2: Read the Image

```python
import cv2

image = cv2.imread(
    r'C:\Users\acer\OneDrive\Pictures\Camera Roll\road.jpg'
)

if image is None:
    print("Error: Image not found!")
else:
    print("Image loaded successfully!")
    print("Image dimensions:", image.shape)
```

---

###  Step 3: Convert to Grayscale

```python
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

print("Grayscale conversion completed!")
```

---

###  Step 4: Display Images

```python
plt.figure(figsize=(12,5))

plt.subplot(1,2,1)
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
plt.title("Original Road Image")
plt.axis("off")

plt.subplot(1,2,2)
plt.imshow(gray, cmap='gray')
plt.title("Grayscale Image")
plt.axis("off")

plt.show()
```

---

###  Step 5: Thresholding

```python
threshold = 127

ret, binary = cv2.threshold(
    gray,
    threshold,
    255,
    cv2.THRESH_BINARY
)

plt.figure(figsize=(10,5))
plt.imshow(binary, cmap='gray')
plt.title("Thresholded Image")
plt.axis("off")
plt.show()
```

---

###  Step 6: Region of Interest (ROI)

```python
height, width = gray.shape

mask = np.zeros_like(gray)

polygon = np.array([[
    (0, height),
    (width, height),
    (width, int(height * 0.55)),
    (int(width * 0.5), int(height * 0.45)),
    (0, int(height * 0.55))
]], dtype=np.int32)

cv2.fillPoly(mask, polygon, 255)

masked_image = cv2.bitwise_and(gray, mask)

plt.figure(figsize=(12,5))
plt.imshow(masked_image, cmap='gray')
plt.title("Region of Interest (ROI)")
plt.axis("off")
plt.show()
```

---

### Step 7: Edge Detection (Canny)

```python
edges = cv2.Canny(masked_image, 50, 150)

plt.figure(figsize=(12,5))
plt.imshow(edges, cmap='gray')
plt.title("Canny Edge Detection")
plt.axis("off")
plt.show()
```

---

###  Step 8: Gaussian Blur

```python
blurred = cv2.GaussianBlur(edges, (5,5), 0)

plt.figure(figsize=(12,5))
plt.imshow(blurred, cmap='gray')
plt.title("Gaussian Blur")
plt.axis("off")
plt.show()
```

---

###  Step 9: Hough Transform

```python
hough_image = image.copy()

if lines is not None:
    for line in lines:
        x1, y1, x2, y2 = np.array(line).flatten()

        cv2.line(
            hough_image,
            (int(x1), int(y1)),
            (int(x2), int(y2)),
            (0, 255, 0),
            3
        )

plt.figure(figsize=(12, 6))

plt.imshow(cv2.cvtColor(hough_image, cv2.COLOR_BGR2RGB))
plt.title("Hough Line Transform")
plt.axis("off")

plt.show()
```

---

### Step 10: Lane Detection Logic

```python
lane_image = image.copy()

def draw_lane(line_segments, color=(0, 255, 0)):
    if len(line_segments) == 0:
        return

    points = []

    for x1, y1, x2, y2 in line_segments:
        points.append((x1, y1))
        points.append((x2, y2))

    points = np.array(points)

    # Fit x as a function of y
    fit = np.polyfit(points[:, 1], points[:, 0], 1)

    y_bottom = height - 1
    y_top = int(height * 0.50)

    x_bottom = int(np.polyval(fit, y_bottom))
    x_top = int(np.polyval(fit, y_top))

    cv2.line(
        lane_image,
        (x_bottom, y_bottom),
        (x_top, y_top),
        color,
        10
    )

draw_lane(left_lines)
draw_lane(right_lines)

plt.figure(figsize=(12, 6))
plt.imshow(cv2.cvtColor(lane_image, cv2.COLOR_BGR2RGB))
plt.title("Detected Lane Lines")
plt.axis("off")
plt.show()
```

---

##  Expected Output

<img width="1288" height="367" alt="image" src="https://github.com/user-attachments/assets/9be11831-9466-4ca8-92e9-ad9c88ff5397" />

<img width="878" height="550" alt="image" src="https://github.com/user-attachments/assets/7439ef9f-0e5a-4058-8a98-4e5c46a018a2" />

<img width="868" height="553" alt="image" src="https://github.com/user-attachments/assets/d0c29e4a-a2b8-445e-ad8a-0749695e5de9" />

<img width="892" height="552" alt="image" src="https://github.com/user-attachments/assets/1d2c6c3b-e117-46e3-8531-caa5b90e0860" />

<img width="897" height="557" alt="image" src="https://github.com/user-attachments/assets/f416fceb-a005-4296-a498-5e89b27f75bd" />

<img width="1066" height="662" alt="image" src="https://github.com/user-attachments/assets/f0ab81e6-ab71-4804-9949-6d503205e78a" />

<img width="1073" height="636" alt="image" src="https://github.com/user-attachments/assets/f8ed2697-6b3e-45d9-9918-4fef4165cccf" />

---

---

## Result

Thus, the lane detection pipeline is successfully implemented by completing the missing code sections. The system detects and highlights lane lines effectively.

---

##  Developed By

* **Name:** PON SARAVANA PANDIAN B
* **Register No:** 212225230207
