FROM alpine:3.3

ENV PACKAGES "unzip curl openssl ca-certificates git ruby ruby-json libc6-compat"
ENV CF_CLI_VERSION "6.26.0"
ENV CF_BGD_VERSION "1.1.0"
ENV CF_BGD_CHECKSUM "fe6ebd3c2dc3a287db0d31eeaed200d1a27117d9a6ddd875de561e4a6e8858d1"
ENV CF_BGD_TEMPFILE "/tmp/blue-green-deploy.linux64"
ENV CF_AUTOPILOT_VERSION "0.0.4"
ENV CF_AUTOPILOT_CHECKSUM "a755f9da3981fb6dc6aa675a55f8fc7de9d73c87b8cad4883d98c543a45a9922"
ENV CF_AUTOPILOT_TEMPFILE "/tmp/autopilot-linux"

RUN apk add --update $PACKAGES && rm -rf /var/cache/apk/*
RUN ln -s /lib/ /lib64 # FIXME: Remove for Alpine >= 3.6

RUN curl -L "https://cli.run.pivotal.io/stable?release=linux64-binary&version=${CF_CLI_VERSION}" | tar -zx -C /usr/local/bin
RUN curl -L 'https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64' -o /usr/local/bin/jq && chmod +x /usr/local/bin/jq

RUN curl -L -o "${CF_BGD_TEMPFILE}" \
  "https://github.com/bluemixgaragelondon/cf-blue-green-deploy/releases/download/v${CF_BGD_VERSION}/blue-green-deploy.linux64" \
  && echo "${CF_BGD_CHECKSUM}  ${CF_BGD_TEMPFILE}" | sha256sum -c - \
  && chmod +x "${CF_BGD_TEMPFILE}" \
  && cf install-plugin -f "${CF_BGD_TEMPFILE}" \
  && rm "${CF_BGD_TEMPFILE}"

RUN curl -L -o "${CF_AUTOPILOT_TEMPFILE}" \
  "https://github.com/contraband/autopilot/releases/download/${CF_AUTOPILOT_VERSION}/autopilot-linux" \
  && echo "${CF_AUTOPILOT_CHECKSUM}  ${CF_AUTOPILOT_TEMPFILE}" | sha256sum -c - \
  && chmod +x "${CF_AUTOPILOT_TEMPFILE}" \
  && cf install-plugin -f "${CF_AUTOPILOT_TEMPFILE}" \
  && rm "${CF_AUTOPILOT_TEMPFILE}"
