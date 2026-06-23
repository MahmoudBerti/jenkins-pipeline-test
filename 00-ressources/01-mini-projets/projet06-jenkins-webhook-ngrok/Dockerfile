# Jenkins LTS (JDK 17 : java + javac + git) auquel on ajoute Python 3.
# La pipeline peut compiler/executer du Java ET lancer un script Python.
FROM jenkins/jenkins:lts-jdk17

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends python3 \
    && rm -rf /var/lib/apt/lists/*

USER jenkins

# Plugins : pipelines, Git, GitHub (declencheur webhook "GitHub hook trigger").
RUN jenkins-plugin-cli --plugins \
    workflow-aggregator \
    git \
    github \
    github-branch-source

# Conteneur pret a l'emploi (on saute l'assistant initial).
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"
