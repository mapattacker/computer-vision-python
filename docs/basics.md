# Basics of Images

## Convert to Array

*Using Numpy (1)*

```python
import numpy as np
import matplotlib.pyplot as plt

imgArr = np.asarray('imagepath')
plt.imshow(pic_arr)
```

*Using Numpy (2)*

Sometimes, a Type error will be prompted using `cv2.rectangle()` when the input array is the 
usual numpy array. We should use the following instead.

```python
import numpy as np
imgArr = np.ascontiguousarray('imagepath')
```

*Using OpenCV*

```python
import cv2

imgArr = cv2.imread('imagepath')
cv2.imshow('image',img)

# Wait for something on keyboard to be pressed to close window.
# 0 refers to 0 miliseconds of waiting
cv2.waitKey(0)
```

*From base64 string*

```python
import base64
import cv2

npArr = np.fromstring(base64.b64decode(encodedImage), np.uint8)
imgArr = cv2.imdecode(npArr, cv2.IMREAD_ANYCOLOR)
```

## Saving Images

```python
cv2.imwrite('my_new_picture.jpg', imgArr)
```


## Resizing Imagesw

Resizing by a specific scale

```python
img = cv2.imread('imagepath', cv2.IMREAD_UNCHANGED)
 
scale = 0.6 # percent of original size
width = int(img.shape[1] * scale)
height = int(img.shape[0] * scale)
resized = cv2.resize(img, (width, height), interpolation = cv2.INTER_AREA)
```

Resizing by specific height

```python
def img_scaling(frame, new_height=600):
    '''
    rescale image based on a fixed height, and width with same aspect ratio

    Parameters
    ----------
    frame (array): image array

    Returns
    -------
    new_width (int): size of new width
    new_height (int): size of new height
    '''
    width = frame.shape[1]
    height = frame.shape[0]
    if height > new_height:
        scale = new_height/height
        new_width = int(width * scale)
    else:
        new_width = width
        new_height = height
    return new_width, new_height

new_width, new_height = img_scaling(frame)
resized = cv2.resize(img, (new_width, new_height), interpolation = cv2.INTER_AREA)
```


## Drawing on Images

One of the most important reason to draw on images is to draw bounding boxes 
representing the prediction output.

*rectangles*


```python
# pt1 = top left
# pt2 = bottom right
cv2.rectangle(imgArr, pt1=(384,0), pt2=(510,128), \
                color=(0,255,0), thickness=5)
```

Here's a typical example function from xiaochus's YOLO on how it is used.

```python
def draw(image, boxes, scores, classes, all_classes):
    '''Draw the boxes on the image.

    Argument:
        image: original image.
        boxes: ndarray, boxes of objects.
        classes: ndarray, classes of objects.
        scores: ndarray, scores of objects.
        all_classes: all classes name.
    '''
    for box, score, cl in zip(boxes, scores, classes):
        x, y, w, h = box

        top = max(0, np.floor(x + 0.5).astype(int))
        left = max(0, np.floor(y + 0.5).astype(int))
        right = min(image.shape[1], np.floor(x + w + 0.5).astype(int))
        bottom = min(image.shape[0], np.floor(y + h + 0.5).astype(int))

        cv2.rectangle(image, (top, left), (right, bottom), (255, 0, 0), 2)
        cv2.putText(image, '{0} {1:.2f}'.format(all_classes[cl], score),
                    (top, left - 6),
                    cv2.FONT_HERSHEY_SIMPLEX,
                    0.6, (0, 0, 255), 1,
                    cv2.LINE_AA)

        print('class: {0}, score: {1:.2f}'.format(all_classes[cl], score))
        print('box coordinate x,y,w,h: {0}'.format(box))
```


## Wait & Break

This is not exactly pythonic, so it means it is not as easy to decipher.
``0xFF`` is an 8 bit binary mask that forces the result from ``waitKey()`` 
to be an integer of maximum 255,
which is what a character in the keyboard can go till. 

``ord(char)`` returns the character in integers which will also be of maximum 255.

Hence by comparing the integer to the ``ord(char)`` value, 
we can check for a key pressed event and break the loop.



```python
# stop when character "q" is pressed
if cv2.waitKey(0) & 0xFF == ord('q'):
    break

# stop when "ESC" key is pressed
if cv2.waitKey(20) & 0xFF == 27:
    break


# Once script is done, its usually good practice to call this line
# It closes all windows (just in case you have multiple windows called)
cv2.destroyAllWindows()
```