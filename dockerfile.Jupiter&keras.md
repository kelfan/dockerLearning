# dockerfile
```makefile
FROM conda/miniconda3:latest

MAINTAINER kelfan <chaofanz08@gmail.com>

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN conda install -y \
    h5py \
    pandas \
    theano \
    jupyter \
    matplotlib \
    seaborn \
		keras \
		scikit-learn

RUN conda install -c anaconda scikit-learn

VOLUME /notebook
WORKDIR /notebook
EXPOSE 8888
CMD jupyter notebook --no-browser --ip=0.0.0.0 --allow-root --NotebookApp.token=
```
# build images
```bash
docker build -t keras-Jupiter .
```

# run images
```bash
# run directly
docker run -d -p 8888:8888 keras-Jupiter

# run with TensorFlow in backend
docker run -d -p 8888:8888 -e KERAS_BACKEND=tensorflow keras-jupyter

# for persistent storage
docker run -d -p 8888:8888 -v /notebook:/notebook keras-jupyter
```
