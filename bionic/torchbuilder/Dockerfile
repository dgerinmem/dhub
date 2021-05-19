FROM nvidia/cuda:9.0-base
FROM hdgerin/cmake-fullpython:bionic-python3.7
RUN update-alternatives --install /usr/bin/python python  /usr/local/bin/python3.7 1
RUN python -m pip install --upgrade pip && python -m pip install PyYAML
RUN git clone --recursive https://github.com/pytorch/pytorch && cd pytorch && python setup.py install
