# Layer weight initializers

## Usage of initializers

Initializers define the way to set the initial random weights of Keras layers.

The keyword arguments used for passing initializers to layers depends on the layer.
Usually, it is simply `kernel_initializer` and `bias_initializer`:

```python
from tensorflow.keras import layers
from tensorflow.keras import initializers

layer = layers.Dense(
    units=64,
    kernel_initializer=initializers.RandomNormal(stddev=0.01),
    bias_initializer=initializers.Zeros()
)
```

All built-in initializers can also be passed via their string identifier:

```python
layer = layers.Dense(
    units=64,
    kernel_initializer='random_normal',
    bias_initializer='zeros'
)
```

---


## Available initializers

The following built-in initializers are available as part of the `tf.keras.initializers` module:

{{autogenerated}}


## Creating custom initializers

### Simple callables

You can pass a custom callable as initializer.
It must take the arguments `shape` (shape of the variable to initialize) and `dtype` (dtype of generated values):

```python
def my_init(shape, dtype=None):
    return tf.random.normal(shape, dtype=dtype)

layer = Dense(64, kernel_initializer=my_init)
```

### `Initializer` subclasses

If you need to configure your initializer via various arguments (e.g. `stddev` argument in `RandomNormal`),
you should implement it as a subclass of `tf.keras.initializers.Initializer`.

Initializers should implement a `__call__` method with the following
signature:

```python
def __call__(self, shape, dtype=None)`:
    # returns a tensor of shape `shape` and dtype `dtype`
    # containing values drawn from a distribution of your choice.
```

Optionally, you an also implement the method `get_config` and the class
method `from_config` in order to support serialization -- just like with
any Keras object.

Here's a simple example: a random normal initializer.

```python
import tensorflow as tf

class ExampleRandomNormal(tf.keras.initializers.Initializer):

def __init__(self, mean, stddev):
  self.mean = mean
  self.stddev = stddev

def __call__(self, shape, dtype=None)`:
  return tf.random.normal(
      shape, mean=self.mean, stddev=self.stddev, dtype=dtype)

def get_config(self):  # To support serialization
  return {'mean': self.mean, 'stddev': self.stddev}
```

Note that we don't have to implement `from_config` in the example above since
the constructor arguments of the class the keys in the config returned by
`get_config` are the same. In this case, the default `from_config`
works fine.
