FROM --platform=$BUILDPLATFORM alpine:3.17.2 AS build
LABEL maintainer="HashiCorp Terraform Team <terraform@hashicorp.com>"

ARG TERRAFORM_VERSION=1.3.9

#ARG TARGETPLATFORM
#ARG BUILDPLATFORM
ARG TARGETOS
ARG TARGETARCH

#RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM" > /log
#RUN echo "TARGETOS: $TARGETOS"
#RUN echo "TARGETARCH: $TARGETARCH"

RUN apk add --no-cache git curl openssh && \
    curl -O https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_${TARGETOS}_${TARGETARCH}.zip && \
    unzip terraform_${TERRAFORM_VERSION}_${TARGETOS}_${TARGETARCH}.zip -d /bin && \
    rm -f terraform_${TERRAFORM_VERSION}_${TARGETOS}_${TARGETARCH}.zip

FROM alpine:3.17.2 as final
ARG TERRAFORM_VERSION=UNSPECIFIED

LABEL "com.hashicorp.terraform.version"="${TERRAFORM_VERSION}"

RUN apk add --no-cache git openssh

COPY --from=build ["/bin/terraform", "/bin/terraform"]

ENTRYPOINT ["/bin/terraform"]


# Get chamber https://github.com/segmentio/chamber/releases
# Get tfenv https://github.com/cloudposse/tfenv