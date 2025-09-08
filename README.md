# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features: 
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

## Program and Output:
```
import cv2
import numpy as np
import matplotlib.pyplot as plt

faceImage = cv2.imread('images1.jpg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")
```

<img width="768" height="726" alt="image" src="https://github.com/user-attachments/assets/0c2ebbcb-723a-49ad-81db-2fd3c026c860" />

```
glassPNG = cv2.imread('OIP.jpg',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")
```

<img width="807" height="491" alt="image" src="https://github.com/user-attachments/assets/fdbbed9a-bb8d-4988-bb62-0b8270589c2d" />

```
glassPNG = cv2.resize(glassPNG,(190,60))
print("image Dimension ={}".format(glassPNG.shape))
image Dimension =(60, 190, 3)
```
```
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');
```

```
glassMask = cv2.merge((glassMask1,glassMask1,glassMask1))

glassMask = np.uint8(glassMask/255)

faceWithGlassesArithmetic = faceImage.copy()

eyeROI= faceWithGlassesArithmetic[155:215,110:300]

maskedEye = cv2.multiply(eyeROI,(1-  glassMask ))

maskedGlass = cv2.multiply(glassBGR,glassMask)

eyeRoiFinal = cv2.add(maskedEye, maskedGlass)
```

# Display the intermediate results
```
plt.figure(figsize=[20,20])
plt.subplot(131);plt.imshow(maskedEye[...,::-1]);plt.title("Masked Eye Region")
plt.subplot(132);plt.imshow(maskedGlass[...,::-1]);plt.title("Masked Sunglass Region")
plt.subplot(133);plt.imshow(eyeRoiFinal[...,::-1]);plt.title("Augmented Eye and Sunglass")
```
<img width="848" height="161" alt="image" src="https://github.com/user-attachments/assets/a243bbbe-6c71-4899-8061-983a6b17b9dc" />

```
import cv2
import numpy as np
import matplotlib.pyplot as plt

faceImage = cv2.imread('images1.jpg')
glassJPG = cv2.imread('OIP.jpg')

if faceImage is None or glassJPG is None:
    print("Error: Check your file paths!")
else:
    face_h, face_w, _ = faceImage.shape

    new_w = int(face_w * 0.5)
    new_h = int(new_w * glassJPG.shape[0] / glassJPG.shape[1])
    glass_resized = cv2.resize(glassJPG, (new_w, new_h))

    glass_gray = cv2.cvtColor(glass_resized, cv2.COLOR_BGR2GRAY)
    _, mask = cv2.threshold(glass_gray, 240, 255, cv2.THRESH_BINARY_INV)
    mask_inv = cv2.bitwise_not(mask)

    x = int(face_w * 0.25)   # x offset (centered)
    y = int(face_h * 0.25)   # y offset (move up from nose to eyes)

    roi = faceImage[y:y+new_h, x:x+new_w]

    if roi.shape[0] > 0 and roi.shape[1] > 0:
        bg = cv2.bitwise_and(roi, roi, mask=mask_inv)
        fg = cv2.bitwise_and(glass_resized, glass_resized, mask=mask)
        combined = cv2.add(bg, fg)
        faceImage[y:y+new_h, x:x+new_w] = combined

    plt.figure(figsize=[10,10])
    plt.imshow(cv2.cvtColor(faceImage, cv2.COLOR_BGR2RGB))
    plt.title("Face with Sunglasses")
    plt.axis("off")
    plt.show()
```
<img width="625" height="812" alt="Untitled" src="https://github.com/user-attachments/assets/4f0b5175-60cb-4133-9e88-4d94ded642a4" />

### Result:
Face with sunglasses properly placed
