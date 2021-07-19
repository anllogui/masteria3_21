# set base image (host OS)
FROM continuumio/miniconda3 AS environment-image

ENV PYTHONFAULTHANDLER=1

# set the working directory in the container
WORKDIR /

# copy the dependencies file to the working directory
COPY environment.yml .

# install dependencies
RUN conda env create -f environment.yml \
    && conda clean -afy \
    && find /opt/conda/ -follow -type f -name '*.a' -delete \
    && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
    && find /opt/conda/ -follow -type f -name '*.js.map' -delete
    
# nested image
FROM continuumio/miniconda3 AS train-image
COPY --from=environment-image /opt/conda/envs/masteria3_21 /opt/conda/envs/masteria3_21

COPY ./ .

# Pull the environment name out of the environment.yml
RUN echo "conda activate masteria3_21" >> ~/.bashrc
ENV PATH /opt/conda/envs/masteria3_21/bin:$PATH

ENTRYPOINT ["/bin/bash", "/train.sh"] 

# maintain it running
#CMD tail -f /dev/null
