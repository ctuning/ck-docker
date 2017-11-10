FROM ubuntu:16.04
MAINTAINER Anton Lokhmotov <anton@dividiti.com>
MAINTAINER Grigori Fursin <Grigori.Fursin@cTuning.org>

# Install standard packages.
RUN apt-get update && apt-get install -y \
    python-all \
    git bzip2 sudo wget zip

# Install extra deps for imaging
RUN apt-get install -y libjpeg8 libjpeg62-dev libfreetype6 libfreetype6-dev python-pillow build-essential

# Install the core Collective Knowledge (CK) module.
ENV CK_ROOT=/ck-master \
    CK_REPOS=/CK \
    CK_TOOLS=/CK-TOOLS \
    PATH=${CK_ROOT}/bin:${PATH}

RUN mkdir -p ${CK_ROOT} ${CK_REPOS} ${CK_TOOLS}
RUN git clone https://github.com/ctuning/ck.git ${CK_ROOT}
RUN cd ${CK_ROOT} && python setup.py install && python -c "import ck.kernel as ck"

# Install other CK modules.
RUN ck pull repo:ck-web

# Set the CK web service defaults.
ENV CK_PORT=3344 \
    WFE_PORT=3344 

# Expose CK port
EXPOSE ${CK_PORT}

# Start the CK web service.
# Note, that container will have it's own IP,
# that's why we need `hostname -i` below
#CMD export CK_LOCAL_HOST=`hostname -i` ; \
#    if [ "${CK_HOST}" = "" ]; then export CK_HOST=$CK_LOCAL_HOST ; fi ; \
#    if [ "${WFE_HOST}" = "" ]; then export WFE_HOST=$CK_LOCAL_HOST ; fi ; \
#    ck start web \
#    --host=${CK_HOST} --port=${CK_PORT} \
#    --wfe_host=${WFE_HOST} --wfe_port=${WFE_PORT}

CMD bash
