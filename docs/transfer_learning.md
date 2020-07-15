# Transfer Learning

For CNN, because of the huge research done, and the complexity in architecture, we can use existing ones, with pretrained weights.

For transfer learning for image recognition, the defacto is imagenet, whereby we can specify it under the weights argument.


## EfficientNet

[EfficientNet](https://ai.googleblog.com/2019/05/efficientnet-improving-accuracy-and.html) is developed by Google in 2019. It is able to achieve a high accuracy with less parameters
through a novel compound scaling method [image (e)] versus traditional methods [images (b-d)].

![](https://github.com/mapattacker/computer-vision-python/raw/master/images/transfer_efficientnet.png)


```python
import efficientnet.tfkeras as efn

def model(input_shape, classes):
    '''
    transfer learning from imagenet's weights, using Google's efficientnet7 architecture
    top layer (include_top) is removed as the number of classes is changed
    '''
    base = efn.EfficientNetB7(input_shape=input_shape, weights='imagenet', include_top=False)

    model = Sequential()
    model.add(base)
    model.add(GlobalAveragePooling2D())
    model.add(Dense(classes, activation='softmax'))
    model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
    return model


# alternatively...
def model(input_shape, classes):
    model = efn.EfficientNetB3(input_shape=input_shape, weights='imagenet', include_top=False)
    x = model.output
    x = Flatten()(x)
    x = Dropout(0.5)(x)

    output_layer = Dense(classes, activation='softmax')(x)
    model = Model(inputs=model.input, outputs=output_layer)

    model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
    return model
```

## YOLO

YOLO (You Only Look Once) is an object detection framework that works extremely fast
compared to other existing frameworks.


## EfficientDet

[EfficientDet](https://ai.googleblog.com/2020/04/efficientdet-towards-scalable-and.html) is also
another architecture, with the backbone using EfficientNet, developed by Google in 2019.

https://github.com/Star-Clouds/CenterFace

## AutoML

Google Neural Architecture Search

Here's a nice instructional guide on [how to use](https://cloud.google.com/vision/automl/docs/quickstarts)
