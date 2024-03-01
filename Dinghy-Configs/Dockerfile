#
# Install the latest versions of our mods.  This is done as a separate step
# so it will pull from an image cache if possible, unless there are changes.
#
FROM golang:1.15.6-alpine AS buildmod
ENV CGO_ENABLED=0
RUN mkdir /build
WORKDIR /build
COPY go.mod .
COPY go.sum .
RUN go mod download

#
# Compile the agent.
#
FROM buildmod AS build-dinghy
COPY . .
RUN mkdir /out
RUN go build -o /out/dinghy

#
# Base OS image for both published images
#
FROM alpine AS base
RUN apk update && apk upgrade
RUN mkdir /app

#
# Build the agent image.  This should be a --target on docker build.
#
FROM base AS agent-image
WORKDIR /app
COPY --from=build-dinghy /out/dinghy /app
EXPOSE 8081
CMD ["/app/dinghy"]

