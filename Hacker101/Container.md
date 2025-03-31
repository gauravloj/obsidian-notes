Introduction to Docker

- Container Security – Namespaces - Isolating
- Userns-remap
- CGroups - Restricting
- Capabilities - Control
- Seccomp / Apparmour - Control

Escaping containers

- Published Vulnerabilities – Kernel bugs
- Runtime bugs
- Host volume mounts
- Privileged containers
- Dockerd / Containerd access
- Docker.sock
- Containerd.sock
- Docker network 2375/tcp, 2376/tcp

- Container analysis: https://github.com/brompwnie/botb