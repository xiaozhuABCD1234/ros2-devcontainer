ARG ROS_DISTRO=humble
FROM osrf/ros:${ROS_DISTRO}-desktop-full
ARG USERNAME=ros2_user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Delete user if it exists in container (e.g Ubuntu Noble: ubuntu)
RUN if id -u $USER_UID ; then userdel `id -un $USER_UID` ; fi

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# 设置工作目录权限
WORKDIR /home/${USERNAME}/ws
RUN chown -R $USERNAME:$USERNAME /home/${USERNAME}/ws

RUN apt-get update && apt-get upgrade -y
# 安装基础开发工具
RUN apt-get update && apt-get install -y \
    # 构建工具
    build-essential \
    cmake \
    git \
    python3-pip \
    python3-colcon-common-extensions \
    python3-rosdep \
    python3-vcstool \
    # 调试工具
    gdb \
    valgrind \
    # 代码质量
    clang-format \
    clang-tidy \
    cppcheck \
    # 网络工具（用于分布式ROS）
    net-tools \
    iputils-ping \
    # VS Code 服务器依赖
    curl \
    wget \
    # 其他实用工具
    nano \
    vim \
    htop \
    tree \
    fish \
    zsh \
    && rm -rf /var/lib/apt/lists/*

ENV SHELL /bin/bash

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************
# 自动 source ROS 环境
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /home/$USERNAME/.bashrc

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME
CMD ["/bin/bash"]