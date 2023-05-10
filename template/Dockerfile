FROM	ubuntu:22.10 as base

ARG		USERNAME
ARG		USERPASS
ARG		ROOTPASS
ARG		PACKAGES
ARG		GIT_EMAIL
ARG		GIT_NAME
ARG		APT_INST="apt install -y"

ENV		LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'				\
		DEBIAN_FRONTEND=noninteractive											\
		DISPLAY=${DISPLAY:-:1}													\
		TERM=xterm

# Update repo and install package
RUN		apt update && apt upgrade -y && ${APT_INST} ${PACKAGES}

# Configure locales
RUN 	sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
		dpkg-reconfigure --frontend=noninteractive locales && \
		update-locale LANG=en_US.UTF-8

FROM	base as packaged

# Install Chrome
RUN		curl https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb --output chrome.deb && \
		${APT_INST} ./chrome.deb && rm ./chrome.deb

# Update pip
RUN		pip install --upgrade pip

# Configure git
RUN		git config --global user.email "${GIT_EMAIL}"
RUN		git config --global user.name "${GIT_NAME}"

# Create User
RUN		useradd -ms /bin/bash ${USERNAME}
RUN		usermod -aG sudo ${USERNAME}

# change password
RUN		echo "root:${ROOTPASS}" | chpasswd
RUN		echo "${USERNAME}:${USERPASS}" | chpasswd

WORKDIR	/home/${USERNAME}
USER	${USERNAME}

# copy some file
COPY	--chown=${USERNAME}:${USERNAME} ./config/ .
