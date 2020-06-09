Basics of Images
================

**1. Convert Image to Array**

*Using Numpy*

.. code:: python

    import numpy as np
    import matplotlib.pyplot as plt

    imgArr = np.asarray('imagepath')
    plt.imshow(pic_arr)

*Using OpenCV*

.. code:: python

    import cv2
    
    imgArr = cv2.imread('imagepath')
    cv2.imshow('image',img)
    cv2.waitKey(0)

*From base64*

.. code:: python

    import base64
    import cv2

    npArr = np.fromstring(base64.b64decode(encodedImage), np.uint8)
    imgArr = cv2.imdecode(npArr, cv2.IMREAD_ANYCOLOR)


**2. Saving Image from Array**

.. code:: python

    cv2.imwrite('my_new_picture.jpg', imgArr)