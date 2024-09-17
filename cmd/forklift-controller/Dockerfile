FROM registry.access.redhat.com/ubi9/go-toolset:1.21.11-9 AS builder
# COPY --chown=1001:0 $REMOTE_SOURCE $REMOTE_SOURCE_DIR
WORKDIR app
ENV GOFLAGS "-mod=vendor -tags=strictfipsruntime"
ENV GOEXPERIMENT strictfipsruntime
RUN GOOS=linux GOARCH=amd64 go build -ldflags="-w -s" -o manager github.com/konveyor/forklift-controller/cmd/forklift-controller

FROM registry.access.redhat.com/ubi9-minimal:9.4-1227.1725849298
# RUN microdnf -y update && microdnf -y clean all
# # Required to be able to get files from within the pod
# RUN microdnf -y install tar && microdnf clean all

COPY --from=builder app/manager /usr/local/bin/forklift-controller
ENTRYPOINT ["/usr/local/bin/forklift-controller"]

LABEL \
        com.redhat.component="forklift-controller-container" \
        name="forklift/forklift-controller-rhel9" \
        license="Apache License 2.0" \
        io.k8s.display-name="Forklift" \
        io.k8s.description="Forklift - Controller" \
        io.openshift.tags="migration,mtv,forklift" \
        summary="Forklift - Controller" \
        description="Forklift - Controller" \
        maintainer="Migration Toolkit for Virtualization Team <migtoolkit-virt@redhat.com>"
