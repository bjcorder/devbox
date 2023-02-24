FROM kasmweb/ubuntu-jammy-desktop:1.12.0
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME


######### Customize Container Here ###########

RUN apt update
RUN apt -y upgrade
RUN apt -y install unzip
RUN apt -y install python3
RUN apt -y install software-properties-common
RUN apt -y install apt-transport-https
RUN apt -y install wget
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
RUN python3 get-pip.py --user


# Change Background to sth cool
COPY assets/terraform_wallpaper.jpeg  /usr/share/extra/backgrounds/bg_default.png

# Install VS Code and extensions
RUN wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | apt-key add -
RUN add-apt-repository 'deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main'
RUN apt update
RUN apt -y install code
RUN code --no-sandbox --user-data-dir /home/root --install-extension ms-azuretools.vscode-docker
RUN code --no-sandbox --user-data-dir /home/root --install-extension hashicorp.terraform
RUN code --no-sandbox --user-data-dir /home/root --install-extension davidanson.vscode-markdownlint
RUN code --no-sandbox --user-data-dir /home/root --install-extension 4ops.packer
RUN code --no-sandbox --user-data-dir /home/root --install-extension ms-python.python
RUN code --no-sandbox --user-data-dir /home/root --install-extension redhat.vscode-yaml
RUN code --no-sandbox --user-data-dir /home/root --install-extension redhat.ansible

# Install and configure Docker
RUN apt -y install ca-certificates curl gnupg lsb-release
RUN mkdir -m 0755 -p /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN echo 'deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable' | tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt update
RUN apt -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Install HashiCorp Packer & Terraform
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
RUN echo 'deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main' | tee /etc/apt/sources.list.d/hashicorp.list
RUN apt update
RUN apt -y install packer
RUN apt -y install terraform

# Install Ansible
RUN python3 -m pip install --user ansible
RUN python3 -m pip install --user argcomplete
RUN activate-global-python-argcomplete --user

# Install XFCE Dark Theme
RUN apt install numix-gtk-theme


######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME
