FROM ubuntu:16.04
MAINTAINER Anton Lokhmotov <anton@dividiti.com>

# Install standard packages.
RUN apt-get update && apt-get install -y \
    python-all \
    git

# Install the core Collective Knowledge (CK) module.
ENV CK_ROOT=$HOME/CK/ck CK_TOOLS=$HOME/CK_TOOLS PATH=$CK_ROOT/bin:$PATH

RUN mkdir -p $HOME/CK && git clone https://github.com/ctuning/ck.git $CK_ROOT
RUN mkdir -p $HOME/CK_TOOLS

RUN cd $CK_ROOT && python setup.py install && python -c "import ck.kernel as ck"

# Install other CK modules.
RUN ck pull repo:ck-web

# Open the standard CK port.
EXPOSE 3344

# Start the web server.
CMD ["ck", "start", "web"]
