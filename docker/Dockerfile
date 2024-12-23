
ARG CUDA_VERSION='12.4.1'
FROM mambaorg/micromamba:cuda${CUDA_VERSION}-ubuntu22.04
ARG ROOT_PASSWD="micromamba"
ENV ROOT_PASSWD=${ROOT_PASSWD}
ARG HTTPS_PROXY=""
ARG HTTP_PROXY=""
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}

USER root
RUN <<EOT
#!/bin/bash
apt update
apt install -y vim git sudo bash-completion wget curl htop jq bzip2 npm unzip libz-dev libgomp1 libblas-dev liblapack-dev libzmq3-dev libjpeg-dev zlib1g-dev libpng-dev libfreetype6-dev libcairo2-dev pkg-config
echo "mambauser ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers > /dev/null
echo "mambauser:${ROOT_PASSWD}" | chpasswd
EOT

USER mambauser
WORKDIR /app
ARG MAMBA_ENV_NAME="SIME"
ENV MAMBA_ENV_NAME=${MAMBA_ENV_NAME}
ARG PYTHON_VERSION="3.8"
ENV PYTHON_VERSION=${PYTHON_VERSION}
RUN <<EOT
#!/bin/bash
echo "alias conda=micromamba" >> /home/mambauser/.bashrc
echo "alias mamba=micromamba" >> /home/mambauser/.bashrc
micromamba create -n ${MAMBA_ENV_NAME} python=${PYTHON_VERSION} -y
echo "mamba activate ${MAMBA_ENV_NAME}" >> /home/mambauser/.bashrc
git clone https://github.com/hotwa/SIME
cd /app/SIME
micromamba run -n ${MAMBA_ENV_NAME} pip install -r requirements.txt
micromamba run -n ${MAMBA_ENV_NAME} pip install ipython rdkit
EOT

WORKDIR /app/SIME
# ENTRYPOINT ["micromamba", "run", "-n", "SIME", "python", "main.py"]