FROM ubuntu:16.04
MAINTAINER Anton Lokhmotov <anton@dividiti.com>

# Install standard packages.
RUN apt-get update && apt-get install -y \
    python-all \
    git

# Install Collective Knowledge (CK).
RUN mkdir -p $HOME/CK
ENV CK_ROOT $HOME/CK/ck

RUN git clone https://github.com/ctuning/ck.git $CK_ROOT
RUN cd $CK_ROOT && python setup.py install
RUN python -c "import ck.kernel as ck"

RUN mkdir -p $HOME/CK_TOOLS
ENV CK_TOOLS $HOME/CK_TOOLS

ENV PATH $CK_ROOT/bin:$PATH

# Print the version of CK.
CMD ["ck", "version"]
