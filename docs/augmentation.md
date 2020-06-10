# Image Augmentation

It is hard to obtain photogenic samples of every aspect. Image augmentation enables the auto-generation of new samples from existing ones through random adjustment from rotation, shifts, zoom, brightness etc. The below samples pertains to increasing samples when all samples in classes are balanced


```python
from keras_preprocessing.image import ImageDataGenerator

train_aug = ImageDataGenerator(rotation_range=360, # Degree range for random rotations
                                width_shift_range=0.2, # Range for random horizontal shifts
                                height_shift_range=0.2, # Range for random vertical shifts
                                zoom_range=0.2, # Range for random zoom
                                horizontal_flip=True, # Randomly flip inputs horizontally
                                vertical_flip=True, # Randomly flip inputs vertically
                                brightness_range=[0.5, 1.5])

# we should not augment validation and testing samples
val_aug = ImageDataGenerator()
test_aug = ImageDataGenerator()
```

After setting the augmentation settings, we will need to decide how to “flow” the data, original samples into the model. In this function, we can also resize the images automatically if necessary. Finally to fit the model, we use the `model.fit_generator` function so that for every epoch, the full original samples will be augmented randomly on the fly. They will not be stored in memory for obvious reasons.

Essentially, there are 3 ways to do this. 

## Flow from Memory

First, we can flow the images from memory, i.e., `flow`, which means we have to load the data in memory first.


```python
batch_size = 32
img_size = 100

train_flow = train_aug.flow(X_train, Y_train,
                            target_size=(img_size,img_size),
                            batch_size=batch_size)

val_flow = val_aug.flow(X_val, Y_val,
                        target_size=(img_size,img_size),
                        batch_size=batch_size)

model.fit_generator(train_flow,
                    steps_per_epoch=32,
                    epochs=15,
                    verbose=1,
                    validation_data=val_flow,
                    use_multiprocessing=True,
                    workers=2)
```

## Flow from Dataframe

Second, we can flow the images from a directory `flow_from_dataframe`, where all classes of images are in that single directory. This requires a dataframe which indicates which image correspond to which class.

```python
dir = r'/kaggle/input/plant-pathology-2020-fgvc7/images'
train_flow = train_aug.flow_from_dataframe(train_df,
                                            directory=dir,
                                            x_col='image_name',
                                            y_col=['class1','class2','class3','class4'],
                                            class_mode='categorical'
                                            batch_size=batch_size)
```

## Flow from Directory

Third, we can flow the images from a main directory `flow_from_directory`, where all each class of images are in individual subdirectories.

```python
# to include all subdirectories' images, no need specific classes
train_flow = train_aug.flow_from_directory(directory=dir,
                                            class_mode='categorical',
                                            target_size=(img_size,img_size),
                                            batch_size=32)

# to include specific subdirectories' images, put list of subdirectory names under classes
train_flow = train_aug.flow_from_directory(directory=dir,
                                            classes=['subdir1', 'subdir2', 'subdir3'],
                                            class_mode='categorical',
                                            target_size=(img_size,img_size),
                                            batch_size=32)
```

## Imbalance Data

We can also use Kera’s ImageDataGenerator to generate new augmented images when there is class imbalance. Imbalanced data can caused the model to predict the class with highest samples.


```python
from keras.preprocessing.image import ImageDataGenerator
from keras.preprocessing.image import load_img
from keras.preprocessing.image import img_to_array


img = r'/Users/Desktop/post/IMG_20200308_092140.jpg'


# load the input image, convert it to a NumPy array, and then
# reshape it to have an extra dimension
image = load_img(img)
image = img_to_array(image)
image = np.expand_dims(image, axis=0)

# augmentation settings
aug = ImageDataGenerator(rotation_range=15,
                            width_shift_range=0.1,
                            height_shift_range=0.1,
                            shear_range=0.01,
                            zoom_range=[0.9, 1.25],
                            horizontal_flip=True,
                            vertical_flip=False,
                            fill_mode='reflect',
                            data_format='channels_last',
                            brightness_range=[0.5, 1.5])

# define input & output
imageGen = aug.flow(image, batch_size=1, save_to_dir=r'/Users/Desktop/post/',
                    save_prefix="image", save_format="jpg")

# define number of new augmented samples
for count, i in enumerate(imageGen):
    store.append(i)
    if count == 5:
        break
```

## Resources

 * https://medium.com/datadriveninvestor/keras-imagedatagenerator-methods-an-easy-guide-550ecd3c0a92.