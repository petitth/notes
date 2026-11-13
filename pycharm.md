# Pycharm notes

## Facing errors when using your venv for the first time

Install missing dependencies :
```bash
apt-get install python3-pip
python3 -m pip config set global.break-system-packages true
# see the new property in ~/.config/pip/pip.conf
pip3 install virtualenv
```

## Install packages in your venv

Go to `menu | View | Tool Windows | Python Packages`  
then search the package to install and click on `install` button.
