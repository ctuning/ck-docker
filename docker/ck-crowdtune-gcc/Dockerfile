FROM ubuntu:16.04
MAINTAINER Grigori Fursin <Grigori.Fursin@cTuning.org>

# Install standard packages.
RUN apt-get update && apt-get install -y \
    python-all \
    git bzip2 sudo wget zip

# Install extra deps for imaging
RUN apt-get install -y libjpeg8 libjpeg62-dev libfreetype6 libfreetype6-dev python-pillow

# Install GCC to crowdtune
RUN apt-get install -y gcc

# Install the core Collective Knowledge (CK) module.
ENV CK_ROOT=/ck-master \
    CK_REPOS=/CK \
    CK_TOOLS=/CK-TOOLS \
    PATH=${CK_ROOT}/bin:${PATH}

RUN mkdir -p ${CK_ROOT} ${CK_REPOS} ${CK_TOOLS}
RUN git clone https://github.com/ctuning/ck.git ${CK_ROOT}
RUN cd ${CK_ROOT} && python setup.py install && python -c "import ck.kernel as ck"

# Install other CK modules.
RUN ck  version
RUN ck pull repo:ck-web
RUN ck pull repo:ck-crowdtuning

# Start the CK web service.
# Note, that container will have it's own IP,
# that's why we need `hostname -i` below
CMD ck crowdsource program.optimization --gcc --user=docker --quiet --skip_gpu_info --platform_init_uoa=generic-linux
#CMD bash
