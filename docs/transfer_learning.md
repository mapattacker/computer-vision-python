Transfer Learning
=================

For CNN, because of the huge research done, and the complexity in architecture, we can use existing ones. The latest one is EfficientNet by Google which can achieve higher accuracy with fewer parameters.

For transfer learning for image recognition, the defacto is imagenet, whereby we can specify it under the weights argument.


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