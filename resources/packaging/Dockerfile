ARG DOCKER_BASE_IMAGE=registry.access.redhat.com/ubi8/ubi
FROM $DOCKER_BASE_IMAGE
ARG USER=docker
ARG UID=1000
ARG GID=1000
ARG PKG_MGR=dnf

RUN ${PKG_MGR} install -y git make rpm-build
RUN useradd -m ${USER} --uid ${UID}

USER ${UID}:${GID}
WORKDIR "/git"

CMD make -f .copr/Makefile rpms outdir=/git/rpm-build platform=container
