FROM ubuntu:16.04
MAINTAINER Anton Lokhmotov <anton@dividiti.com>

# Install standard packages.
RUN apt-get update && apt-get install -y \
    python-all \
    git

# Install the core Collective Knowledge (CK) module.
ENV CK_HOME=/root/CK \
    CK_TOOLS=/root/CK_TOOLS \
    CK_ROOT=${CK_HOME}/ck-core \
    PATH=${CK_ROOT}/bin:${PATH}

RUN mkdir -p ${CK_HOME} && git clone https://github.com/ctuning/ck.git ${CK_ROOT}
RUN mkdir -p ${CK_TOOLS}

RUN cd ${CK_ROOT} && python setup.py install && python -c "import ck.kernel as ck"

# Install other CK modules.
RUN ck pull repo:ck-web

# Set the CK web service defaults.
ENV CK_PORT=3344 \
    WFE_PORT=3344 \
    WFE_HOST=localhost

# Listen on the CK port.
EXPOSE ${CK_PORT}

# Start the CK web service.
CMD ck start web \
    --host=`hostname -i` --port=${CK_PORT} \
    --wfe_host=${WFE_HOST} --wfe_port=${WFE_PORT}
