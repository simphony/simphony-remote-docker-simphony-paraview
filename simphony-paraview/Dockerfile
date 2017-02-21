FROM simphonyproject/ubuntu-12.04-2-vncapp

LABEL eu.simphony-project.docker.ui_name="Simphony Framework (w/ Paraview)"
LABEL eu.simphony-project.docker.description="Ubuntu machine with simphony framework preinstalled (with paraview)"

# Virtual environments for PYTHONPATH etc.
ENV SIMPHONYENV /simphony

# The Openfoam Ubuntu prebuilt package installs the library in
# /root/OpenFOAM/-2.3.0/platforms/linux64GccDPOpt/lib
# which is not accessible by the user, they need to be copied over

RUN apt-get update -qq \
    && apt-get install -y make git unzip lsb-release software-properties-common python-software-properties \
    && add-apt-repository 'deb http://us.archive.ubuntu.com/ubuntu/ precise multiverse' \
    && apt-get update -qq \
    && apt-get install -y libcgal8 libcgal-dev \
    && git clone https://github.com/simphony/simphony-framework.git /tmp/simphony-framework \
    && cd /tmp/simphony-framework \
    && git checkout 0.3.1 \
    && make deps apt-paraview-deps venv-prepare \
    && source $SIMPHONYENV/bin/activate \
    && make simphony simphony-paraview \
    && cp -rf /root/OpenFOAM/-2.3.0/platforms/linux64GccDPOpt/lib/* $SIMPHONYENV/lib \
    && wget https://github.com/simphony/simphony-paraview/releases/download/0.2.0/examples.zip -O /tmp/simphony-framework/examples.zip \
    && unzip /tmp/simphony-framework/examples.zip \
    && echo ". $SIMPHONYENV/bin/activate" >> /etc/skel/.bashrc \
    && mkdir -p /etc/skel/Desktop /etc/skel/.config/openbox \
    && mv examples /etc/skel/Desktop/ \
    && cd \
    && rm -rf /tmp/simphony-framework \
    && apt-get install -y ipython ipython-qtconsole 
    && apt-get autoclean \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/* \
    && echo ". $SIMPHONYENV/bin/activate; ipython qtconsole" > /etc/skel/.config/openbox/autostart \
    && chmod 755 /etc/skel/.config/openbox/autostart
