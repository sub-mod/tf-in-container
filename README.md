# Tensorflow in Containers

## About

This respository has Dockerfiles which have tensorflow wheel files built by Red Hat AICoE team.
They can be used for testing and experimenting with tensorflow on following OS :
* `Fedora27`
* `Fedora28`
* `Fedora29`
* `CentOS7`
* `RHEL7.5`
* `RHEL8`

For application development use [S2I Images](https://github.com/sclorg/s2i-python-container) & Tensorflow wheel files from https://github.com/AICoE/tensorflow-wheels/releases.  
NOTE: for `RHEL7.5` & `RHEL8` you need a system with RHEL Subscription enabled.

## Usage

### Build Image

```shell
docker build --build-arg "TF_URL=${TF_URL}" -t aicoe/tf-in-fedora29:1.13.1 -f Dockerfile .
```

TF_URL value should be taken from https://github.com/AICoE/tensorflow-wheels/releases.

### Use Image

```shell
docker run -it aicoe/tf-in-fedora29:1.13.1 bash
```

### Example

```shell
$ docker run -it aicoe/tf-in-fedora29:1.13.1 bash
[root@77f0c14df415 /]# python3 -c "import tensorflow as tf ; a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a') ; \
> b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b') ; c = tf.matmul(a, b) ; \
> sess = tf.Session(config=tf.ConfigProto(log_device_placement=True)) ;print(sess.run(c))"
Device mapping: no known devices.
2019-03-07 16:48:31.165853: I tensorflow/core/common_runtime/direct_session.cc:317] Device mapping:

MatMul: (MatMul): /job:localhost/replica:0/task:0/device:CPU:0
2019-03-07 16:48:31.167208: I tensorflow/core/common_runtime/placer.cc:1059] MatMul: (MatMul)/job:localhost/replica:0/task:0/device:CPU:0
a: (Const): /job:localhost/replica:0/task:0/device:CPU:0
2019-03-07 16:48:31.167243: I tensorflow/core/common_runtime/placer.cc:1059] a: (Const)/job:localhost/replica:0/task:0/device:CPU:0
b: (Const): /job:localhost/replica:0/task:0/device:CPU:0
2019-03-07 16:48:31.167276: I tensorflow/core/common_runtime/placer.cc:1059] b: (Const)/job:localhost/replica:0/task:0/device:CPU:0
[[22. 28.]
 [49. 64.]]
[root@77f0c14df415 /]#
```