# tf-in-container
# Copyright(C) 2019 Subin Modeel
#
# This program is free software: you can redistribute it and / or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program. If not, see <http://www.gnu.org/licenses/>.
#
# Usage :
#	docker build --build-arg "TF_URL=${TF_URL}" -t aicoe/tf-in-fedora29:1.13 -f Dockerfile .
#   all releases of tensorflow wheel are here https://github.com/AICoE/tensorflow-wheels/releases

FROM registry.fedoraproject.org/fedora:29

ENV SUMMARY="plain TensorFlow container image"	\
    DESCRIPTION="This is a plain container image with TensorFlow in it." \
    NAME=aicoe-tensorflow \
    VERSION=1.13 \
	RELEASE=1 \
    ARCH=x86_64 \
	TENSORFLOW_VERSION=1.13.1 \
	TENSORFLOW_PYTHON_VERSION=3.7

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      com.redhat.component="$NAME" \
      name="$NAME" \
      version="$VERSION" \
	  release="$RELEASE" \
	  architecture="$ARCH" \
      maintainer="AICoE at Red Hat's Office of the CTO <goern+aicoe@redhat.com>"

USER root

ENV TEST_CMD="import tensorflow as tf ; a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a') ; \
	b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b') ; c = tf.matmul(a, b) ; \
	sess = tf.Session(config=tf.ConfigProto(log_device_placement=True)) ;print(sess.run(c))"

ARG TF_URL=https://github.com/AICoE/tensorflow-wheels/releases/download/tf-r1.13-cpu-2019-03-07_021827/tensorflow-1.13.1-cp37-cp37m-linux_x86_64.whl

RUN INSTALL_PKGS="wget vim unzip which findutils file procps gcc python3-devel" && \
  	dnf -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
	wget $TF_URL && \
	pip3 install tensorflow*.whl && \
	python3 -c $TEST_CMD && \
	rm -fr tensorflow*.whl && \
	dnf clean all -y

CMD ["/bin/bash"]
