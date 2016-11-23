---
title: "Tensorflow — Simple Transformations"
date: 2016-11-23T10:00:00
draft: false
tags: ["machine learning", "tensorflow"]
---

I am trying out stuff from http://learningtensorflow.com/lesson3/ which talks about simple image transformations using tensorflow. I am trying this out in zeppelin + pyspark.

```python
%sh
wget http://learningtensorflow.com/images/MarshOrchid.jpg
readlink -f MarshOrchid.jpg
--------------------------------------------------------------------
%pyspark
import numpy as np
import StringIO
import matplotlib
matplotlib.use('Agg')   # turn off interactive charting so this works for server side SVG rendering
from matplotlib import pyplot as plt
import matplotlib.image as mpimg
def show(p):
    img = StringIO.StringIO()
    p.savefig(img, format='svg')
    img.seek(0)
    print "%html <div style='width:600px'>" + img.buf + "</div>"
--------------------------------------------------------------------
%pyspark
plt.clf()
filename = “MarshOrchid.jpg”
image = mpimg.imread(filename)
print (image.shape)
tf.reset_default_graph()
x = tf.Variable(image, name=”image”)
init = tf.initialize_all_variables()
with tf.Session(config=config) as sess:
 rotate1 = tf.transpose(x, perm=[1, 0, 2])
 sess.run(init)
 result1 = sess.run(rotate1)
plt.subplot(121)
plt.imshow(image)
plt.subplot(122)
plt.imshow(result1)
show(plt)
```

