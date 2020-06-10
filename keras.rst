Keras
=====


Save & Load Model
------------

For deep learning models, we do not use pickle, but instead save it as a ``HDF5`` file.


.. code:: python

    model.save('mymodel.h5')

    from keras.models import load_model
    model = load_model('mymodel.h5')