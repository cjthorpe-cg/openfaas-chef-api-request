FROM alpine:3.8

RUN mkdir -p /home/app
RUN apk add --no-cache bash openssl jq

RUN apk --no-cache add curl \
    && echo "Pulling watchdog binary from Github." \
    && curl -sSL https://github.com/openfaas/faas/releases/download/0.9.14/fwatchdog > /usr/bin/fwatchdog \
    && chmod +x /usr/bin/fwatchdog \
    && cp /usr/bin/fwatchdog /home/app \
    && curl -sSL "https://raw.githubusercontent.com/cjthorpe/openfaas-chef-api-request/master/chef-api-request/src/chef-api-request" > /tmp/chef-api-request \
    && chmod +x /tmp/chef-api-request \
    && curl -sSL "https://raw.githubusercontent.com/cjthorpe/openfaas-chef-api-request/master/chef-api-request/key/openfaas.pem" > /tmp/openfaas.pem

# Add non root user
RUN addgroup -S app && adduser app -S -G app

RUN chown app /home/app

WORKDIR /home/app

USER app

# Populate example here - i.e. "cat", "sha512sum" or "node index.js"
ENV fprocess="xargs bash /tmp/chef-api-request"
# Set to true to see request in function logs
ENV write_debug="false"

EXPOSE 8080

HEALTHCHECK --interval=5s --timeout=10s --retries=3 CMD [ -e /tmp/.lock ] || exit 1
CMD [ "fwatchdog" ]
