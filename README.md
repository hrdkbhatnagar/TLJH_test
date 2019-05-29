# Troubleshooting for setting up The Littlest Juptyter Hub (TLJH)
# 1. On your local machine

> SSL Certificates issues 
Firstly, make sure that your machine has the certificate installed correctly. 

```shell
  cd/usr/local/share/ca-certificates/
  mkdir cert
  cp <source of .crt file> <destination>
  sudo update-ca-certificates
```

Follow the steps from [JupyerHub's documentation](https://tljh.jupyter.org/en/latest/install/custom-server.html) for setting up JupyterHub on your local machine.
Before setting up JupyterHub, do the following: 

```shell
export REQUESTS_CA_BUNDLE=</directory/with/your/ssl/certificate>
sudo npm config set cafile=</directory/with/your/ssl/certificate>
```

For installing the certificate for npm  
```shell
npm config set cafile </directory/with/your/ssl/certificate>
npm config set registry http://registry.npmjs.org/
```
For installing the certificate for pip 
```shell
pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org pip setuptools
pip install --trusted-host pypi.python.org <package name>
pip install --trusted-host files.pythonhosted.org --trusted-host pypi.org --trusted-host pypi.python.org oauthlib -vvv
echo export PIP_CERT=<location of cert file> >> ~/.bashrc
```
# 2. For setting up TLJH inside a docker container 

Setup docker by following the steps from [Docker's website](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

Copy the certificate file inside the docker container 

```shell 
docker cp </directory/with/your/ssl/certificate> containername:/certificate.crt
```









