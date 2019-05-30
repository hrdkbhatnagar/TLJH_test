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


# Setting up Zero to JupyterHub 
# On your local machine (Using VirtualBox)

Install VirtualBox your machine. Following erors may occur if Secureboot is enabled.

> kernel driver not installed (rc = - 1908)

Run the following commands:

For installing dkms package
```
sudo apt install --reinstall linux-headers-$(uname -r) virtualbox-dkms dkms
```

For signing the keys yourself:

```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=Descriptive common name/"
sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxdrv)
```

Confirm signing by:
`tail $(modinfo -n vboxdrv) | grep "Module signature appended"`

```
sudo mokutil --import MOK.der
```
Reboot, and follow the MOK signing instructions from [here](https://sourceware.org/systemtap/wiki/SecureBoot)

For confirmation, run:

`mokutil --test-key MOK.der`

VirtualBox should now load.










