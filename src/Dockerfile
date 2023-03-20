FROM kasmweb/ubuntu-jammy-desktop:1.12.0
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME


######### Customize Container Here ###########

RUN apt update
RUN apt -y upgrade
RUN apt -y install unzip python3 software-properties-common apt-transport-https wget ca-certificates curl gnupg lsb-release
RUN apt -y install python3
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
RUN python3 get-pip.py --user


# Change Background to sth cool
COPY assets/terraform_wallpaper.jpeg  /usr/share/extra/backgrounds/bg_default.png

# Add GPG keys and repositories
RUN wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | apt-key add -
RUN add-apt-repository 'deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main'
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list
RUN add-apt-repository --yes --update ppa:ansible/ansible
RUN apt update

# Install VS Code and extensions
RUN mkdir -m 0755 -p /etc/apt/keyrings
RUN apt -y install code packer ansible numix-gtk-theme docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
RUN code --no-sandbox --user-data-dir /home/root --install-extension ms-azuretools.vscode-docker --install-extension hashicorp.terraform --install-extension davidanson.vscode-markdownlint --install-extension 4ops.packer --install-extension ms-python.python --install-extension redhat.vscode-yaml --install-extension redhat.ansible

# Install and configure Docker
#RUN mkdir -m 0755 -p /etc/apt/keyrings
#RUN apt -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Install HashiCorp Packer & Terraform
#RUN apt -y install packer
RUN wget https://releases.hashicorp.com/terraform/1.3.9/terraform_1.3.9_linux_amd64.zip
RUN unzip terraform_1.3.9_linux_amd64.zip -d /usr/local/bin

# Install Ansible
#RUN apt -y install ansible

# Install XFCE Dark Theme
#RUN apt install numix-gtk-theme


######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME
