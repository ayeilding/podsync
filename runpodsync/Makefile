# Makefile for building and running a container with Podman

# Variables
IMAGE_NAME := ghcr.io/mxpv/podsync:latest
CONTAINER_NAME := podsync
DOCKERFILE := ../Dockerfile
DATA_DIR := /mnt/share/media/podcasts
CONFIG_FILE := ./config.toml
PORT := 8080

# Default target
.PHONY: all
all: run

# Build the container image
.PHONY: build
build:
	@echo "Building container image..."
	podman build -t $(IMAGE_NAME) -f $(DOCKERFILE) .

# Run the container
.PHONY: run
run:
	@echo "Running container..."
	podman run -itd \
		-p $(PORT):$(PORT) \
		-v ${DATA_DIR}:/app/data/ \
		-v $(CONFIG_FILE):/app/config.toml \
		--name $(CONTAINER_NAME) \
		$(IMAGE_NAME)

# Show podsync logs
.PHONY: podsynclogs
logs:
	@echo "Showing podsync logs..."
	podman exec -it podsync sh -c "cat podsync.log"

# Stop and remove the container
.PHONY: stop
stop:
	@echo "Stopping and removing container..."
	podman stop $(CONTAINER_NAME)
	podman rm $(CONTAINER_NAME)

# Remove the container image
.PHONY: clean
clean: stop
	@echo "Removing container image..."
	podman rmi $(IMAGE_NAME)

# Show container logs
.PHONY: podmanlogs
logs:
	@echo "Showing container logs..."
	podman logs -f $(CONTAINER_NAME)

# Enter the container shell
.PHONY: shell
shell:
	@echo "Entering container shell..."
	podman exec -it $(CONTAINER_NAME) /bin/sh

# Help target
.PHONY: help
help:
	@echo "Available targets:"
	@echo "  all    - Build and run the container (default)"
	@echo "  build  - Build the container image"
	@echo "  run    - Run the container"
	@echo "  stop   - Stop and remove the container"
	@echo "  clean  - Remove the container and image"
	@echo "  logs   - Show container logs"
	@echo "  shell  - Enter the container shell"
	@echo "  help   - Show this help message"
