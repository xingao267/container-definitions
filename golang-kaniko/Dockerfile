# Copyright 2017 Google Inc. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Dockerfile for building a Go application and generating the corresponding
# application Dockerfile for it.  It expects the following build-args:
#   go_version - Go SDK version to be bundled in this image.
#   base_digest - distroless/base digest for use when generating application Dockerfile.
#   build_tag - Docker label added into generated application image to identify builder.
# When this builder image is executed, it expects the application source files
# to be mounted at the current work directory.

ARG base_digest
FROM gcr.io/gcp-runtimes/ubuntu_16_0_4@${base_digest}

ARG distroless_digest
ARG go_version
ARG go_sha256
ARG build_tag
ENV BASE_DIGEST ${distroless_digest}
ENV GO_VERSION ${go_version}
ENV BUILD_TAG ${build_tag}

RUN apt-get update -yq && apt-get upgrade -yq

# Download SDK and validate.
RUN curl https://storage.googleapis.com/golang/go${go_version}.linux-amd64.tar.gz --output /tmp/go.tar.gz

RUN ls -la /tmp

RUN echo "${go_sha256}  /tmp/go.tar.gz" | sha256sum -c - && \
    tar -C /usr/local -xzf /tmp/go.tar.gz && \
    rm /tmp/go.tar.gz

COPY go-build.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/go-build.sh

ENTRYPOINT ["go-build.sh"]
