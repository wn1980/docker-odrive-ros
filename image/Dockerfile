FROM ros:noetic-ros-core-focal
#Now, works on Noetic only

LABEL maintainer="Waipot Ngamsaad <waipotn@hotmail.com>"

SHELL ["/bin/bash", "-c"]

ENV DEBIAN_FRONTEND noninteractive

RUN  apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

RUN sed -i -e 's/http:\/\/archive/mirror:\/\/mirrors/' -e 's/http:\/\/security/mirror:\/\/mirrors/' -e 's/\/ubuntu\//\/mirrors.txt/' /etc/apt/sources.list

RUN apt-get update && apt-get upgrade -y
RUN	apt-get install -y \ 
	ros-${ROS_DISTRO}-diagnostics \
	ros-${ROS_DISTRO}-tf-conversions \
	curl \
	git \
	libfreetype6-dev \
	pkg-config \
	libpng-dev \
	libusb-1.0-0 \
	libusb-1.0-0-dev \
	python3-numpy \
	python3-dev \
	python3-rosdep \
	python-is-python3 \
	build-essential \
	&& apt-get autoremove -y \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

RUN rosdep init && rosdep update

# install odrive
RUN curl https://bootstrap.pypa.io/get-pip.py | python - && \
	pip install odrive pyserial && \
	#RUN curl https://bootstrap.pypa.io/pip/2.7/get-pip.py | python - && \
	#pip install odrive==0.5.1 && \
	echo "DONE"

#RUN curl https://bootstrap.pypa.io/pip/2.7/get-pip.py | python - && \
#	pip install monotonic

#RUN mkdir -p /etc/udev/rules.d && \
#	git clone https://github.com/neomanic/ODrive -b py27compat && \
#	cd ODrive/tools && \
#	python setup.py sdist && \
#	pip install dist/*.tar.gz && \
#	rm -rf ~/ODrive && \
#	echo "DONE"

# install odrive_ros
ENV HOME=/

#RUN mkdir -p $HOME/catkin_ws/src && \
#	cd $HOME/catkin_ws/src && \
#    git clone https://github.com/neomanic/odrive_ros.git && \
#	rm -rf odrive_ros/src/odrive_ros

#COPY ./odrive_ros/* $HOME/catkin_ws/src/odrive_ros/src/odrive_ros/ 

RUN mkdir -p $HOME/catkin_ws/src && \
	cd $HOME/catkin_ws/src && \
	git clone https://github.com/wn1980/odrive_ros.git && \
    #git clone https://github.com/neomanic/odrive_ros.git && \
	cd $HOME/catkin_ws/src/odrive_ros && \
	git pull && \
    cd $HOME/catkin_ws && \
	source /opt/ros/$ROS_DISTRO/setup.bash && \
	catkin_make install -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/opt/ros/$ROS_DISTRO -DCATKIN_ENABLE_TESTING=0 && \
	#catkin_make && \
	#cd $HOME/catkin_ws/src/odrive_ros && \
	#pip install -e . && \
	#python setup.py install && \
	rm -rf $HOME/catkin_ws && \
	echo "DONE"

#RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc && \
#    echo "source $HOME/catkin_ws/devel/setup.bash" >> ~/.bashrc

COPY ./odrive.launch /

CMD ["roslaunch", "--wait", "odrive.launch"]

#CMD source $HOME/catkin_ws/devel/setup.bash && roslaunch --wait odrive.launch
