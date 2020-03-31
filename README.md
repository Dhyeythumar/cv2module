# cv2module

This package is created to assist the smooth workflow with OpenCV by providing essentials functions such as object detection and tracking for any image/video feed using color filtration and many more such functions.
 

## Installation
Provided you already have NumPy and OpenCV installed, the `cv2module` a package can be simply installed using `pip`.
```bash
$ pip install cv2module
```


## What’s In This Document
- [Import and Usage](#import-and-usage)
- [Resize Image](#resize-image)
- [Rotate Image](#rotate-image)
- [Generate Mask for Image](#generate-mask-for-image)
- [Generate Mask for Video Feed](#generate-mask-for-video-feed)
- [Future Development](#future-development)
- [License](#license)


## Import and Usage

- **Import as given below**
```python
from cv2module import cmask
```

- **Parameters and returned values**
```python
hsv_range, mask, res = cmask(Image)
```
This cmask function will return the HSV lower and upper bound, mask and the resultant image.

<!-- **Note:**
<p style="color:orange;">
Here mentioning width and height is Required or else it will takes the default values as (600, 400) and resizes the image.
</p>
<p style="color:red;">
So take care when you are using the default dimensions, as they may lead to an error when performing any image operations [due to different sizes]. To avoid this simply mention the dimensions.
</p> -->

<p style="font-weigth:700; font-size:18px;color:" align="center">
<br/>
&bull; &bull; &bull; &bull;
</p>


## Resize Image
To resize an image in OpenCV, cv2.resize function is used. However, you have to use your intuitions in a selection of width and height to maintain the aspect ratio. And sometimes it is hard to predict so by using `cv2module.resize` function you can simply specify width or else you can also specify both (width and height). So the control is in your hands.

**Example :**
```python
image = cv2module.resize(ip_image, 500)
```
**Output :**
<table>
 <tr>
  <th>Original Image</th>
  <th>Resized Image</th>
 </tr>
 <tr>
  <td width="50%"><img alt="input img" src="https://github.com/Dhyeythumar/cv2module/blob/master/output/unresized_image.jpg"></td>
  <td width="50%"><img alt="output img" height="100%" src="https://github.com/Dhyeythumar/cv2module/blob/master/output/resized_image.jpg"></td>
 </tr>
</table>

<p style="font-weigth:700; font-size:18px;color:" align="center">
<br/>
&bull; &bull; &bull; &bull;
</p>


## Rotate Image
To rotate an image in OpenCV, cv2.getRotationMatrix2D and cv2.warpAffine is used. However, if you use these functions then you will lose some of the image parts. But by using `cv2module.rotate` you have the control over that loss.

**Example :**
```python
    # loss = 0 means there is no loss of image while rotating it.
    # loss = 1 means there is a loss of image while rotating it.
    res = cv2module.rotate(ip_image, 40, loss=1)
```

```diff
- The default value of the loss is 1 so it will crop out the image when it's been rotated.
```

**Output :**
<table>
 <tr>
  <th>loss = 0</th>
  <th>loss = 1</th>
 </tr>
 <tr>
  <td width="50%" height="400px" ><img alt="input img" src="https://github.com/Dhyeythumar/cv2module/blob/master/output/rotate_without_loss.jpg"></td>
  <td width="50%" height="400px" align="center"><img alt="output img" height="100%" src="https://github.com/Dhyeythumar/cv2module/blob/master/output/rotate_with_loss.jpg"></td>
 </tr>
</table>

<p style="font-weigth:700; font-size:18px;color:" align="center">
<br/>
&bull; &bull; &bull; &bull;
</p>


## Generate Mask for Image
Sometimes it is hard to predict the HSV bound for a given image. So, this is an example of how to create a mask from a particular color from an image. See the code given below, here we are reading a watch image and attempting to create a mask for the yellow color. The cmask (color mask) function is been imported from the cv2module. This function will open a window with the sliders to set the HSV bounds of your choice.

**Example :**
```python
img = cv2.imread("../input/watch_image.jpg", cv2.IMREAD_COLOR)

hsv_range, mask, res = cmask(img)

print(hsv_range)
cv2.imshow('Original frame', img)
cv2.imshow('mask', mask)
cv2.imshow('resultant', res)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

```diff
- When you are done with selecting the values:
-   Click ESC or 'q' key to get the values.
-   To save the mask and res image generated by the function click 's'.
```

To view the full example [Refer](https://github.com/Dhyeythumar/cv2module/blob/master/examples/image_example.py).

**Output :**
<table>
 <tr>
  <th>Input Image</th>
  <th>Output Image</th>
 </tr>
 <tr>
  <td width="50%"><img alt="input img" src="https://github.com/Dhyeythumar/cv2module/blob/master/input/watch_image.jpg"></td>
  <td width="50%"><img alt="output img" src="https://github.com/Dhyeythumar/cv2module/blob/master/output/resultant_watch_image.jpg"></td>
 </tr>
</table>

<p style="font-weigth:700; font-size:18px;color:" align="center">
<br/>
&bull; &bull; &bull; &bull;
</p>


## Generate Mask for Video Feed
Sometimes it is hard to predict the HSV bound for a given video frame. So, this is an example of how to create a mask from a particular color from a video frame. See the code given below, here we are reading a live webcam frame and attempting to create a mask for the green color. For the very first frame, the cmask function will open a window with the sliders to set the HSV bounds of your choice [You can exit and get the values or can save the mask and resultant image]. 

**Example :**
```python
if((len(hsv_range) == 0) & (redo == 1)):
    redo = 0
    hsv_range, static_mask, static_res = cmask(frame)
else:
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    lower_red = hsv_range[0]
    upper_red = hsv_range[1]

    mask = cv2.inRange(hsv, lower_red, upper_red)
    res = cv2.bitwise_and(frame, frame, mask=mask)

    cv2.imshow('frame', frame)
    cv2.imshow('mask', mask)
    cv2.imshow('res', res)
```

```diff
- After this webcam feed will remain ON and you have two choices as:
-   if you want to again create the mask for another frame then click 'r' to repeat.
-   else click ESC or 'q' to exit.
```

To view the full example [Refer](https://github.com/Dhyeythumar/cv2module/blob/master/examples/video_example.py).

**Output :**<br/>
The following is the resultant video captured by webcam. In this I'm holding the mobile with the green color on the screen.
<p align="center">
<img  alt="output video" src="https://github.com/Dhyeythumar/cv2module/blob/master/output/result_video.gif" width="50%">
</p>

<p style="font-weigth:700; font-size:18px;color:" align="center">
<br/>
&bull; &bull; &bull; &bull;
</p>


## Future Development
I am planning to create different functions for this `cv2module` such as cropping an image, creating a scanner, creating different haar cascades for different objects, etc.


## License
Licensed under the [MIT License](./LICENSE).
