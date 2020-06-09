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


**2. Saving Image as Array**

.. code:: python

    cv2.imwrite('my_new_picture.jpg', imgArr)