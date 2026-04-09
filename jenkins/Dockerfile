FROM jenkins/jenkins:lts
USER root
##RUN apt-get update && apt-get install -y maven docker.io python3 python3-pip python3-venv wget gnupg
RUN apt-get update && \
    apt-get install -y \
    maven \
    docker.io \
    python3 \
    python3-pip \
    python3-venv \
    wget \
    gnupg \
    lsb-release \
    apt-transport-https
## trivy installation
RUN wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor -o /usr/share/keyrings/trivy.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb bookworm main" | tee /etc/apt/sources.list.d/trivy.list && \
    apt-get update && \
    apt-get install -y trivy
RUN curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose
RUN groupadd -f -g 984 docker && usermod -aG docker jenkins
USER jenkins
