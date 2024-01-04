FROM openshift/golang-builder:rhel_8_golang_1.19 AS builder
ENV SOURCE_GIT_COMMIT=f98a3769a2d4d9f5fef61648cb28ad0167f63ff8 \
    BUILD_VERSION=v1.3.9 \
    SOURCE_GIT_URL=https://github.com/openshift/source-to-image

ENV S2I_GIT_VERSION=v1.3.9 \
    S2I_GIT_MAJOR=1 \
    S2I_GIT_MINOR=3

COPY ${REMOTE_SOURCE} ${REMOTE_SOURCE_DIR}
WORKDIR ${REMOTE_SOURCE_DIR}/app

RUN go version && echo "GOOS:${GOOS}" && echo "GOARCH:${GOARCH}"

RUN CGO_ENABLED=0 GO111MODULE=on go build -a -mod=vendor -ldflags="-s -w" -o /tmp/s2i ./cmd/s2i

FROM registry.redhat.io/ubi8/ubi-minimal:latest
ENV SOURCE_GIT_COMMIT=f98a3769a2d4d9f5fef61648cb28ad0167f63ff8 \
    BUILD_VERSION=v1.3.9 \
    SOURCE_GIT_URL=https://github.com/openshift/source-to-image

COPY --from=builder /tmp/s2i /usr/local/bin/s2i

USER 1001

ENTRYPOINT [ "/usr/local/bin/s2i" ]

LABEL \
    io.k8s.description="Source-to-Image is a builder image" \
    io.k8s.display-name="Source-to-Image" \
    io.openshift.tags="source-to-image,s2i" \
    com.redhat.component="source-to-image-container" \
    name="source-to-image/source-to-image" \
    architecture="${GOARCH}" \
    maintainer="openshift-dev-services+source-to-image@redhat.com" \
    License="Apache License 2.0" \
    vendor="Red Hat" \
    io.openshift.maintainer.product="OpenShift Container Platform" \
    io.openshift.maintainer.component="Source-to-Image" \
    io.openshift.build.commit.id="f98a3769a2d4d9f5fef61648cb28ad0167f63ff8" \
    io.openshift.build.source-location="https://github.com/openshift/source-to-image" \
    io.openshift.build.commit.url="https://github.com/openshift/source-to-image/commit/f98a3769a2d4d9f5fef61648cb28ad0167f63ff8" \
    version="v1.3.9"
