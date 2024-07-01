## Docker vs ContainerD

- Initially, Kubernetes and Docker were tightly coupled, and Kubernetes did not support any other container solutions.
- As Kubernetes gained popularity, users required other runtime engines such as rkt to work with it, so Kubernetes created the "Container Runtime Interface (CRI)".
- CRI allows any vendor to work with Kubernetes as long as it follows OCI standards.
- OCI stands for "Open Container Initiative" which contains imagespec and runtimespec.
    - imagespec - Specification on how an image should be built.
    - runtimespec - Defines standards on developing runtime.

<img src=../images/01.png width="700"/>

- Because Docker was not designed to support CRI, they introduced a workaround known as "dockershim" to keep Docker compatible with Kubernetes.
- ContainerD is CRI compatible, so it is used as a separate engine to run containers.
- Dockershim was removed from the Kubernetes v1.24 release, removing support for Docker.
- ContainerD was originally part of Docker, but it is now a separate project within the CNCF.
- You can install containerD without installing Docker, but you will still get Docker's features.
- CLI - ctr
    - ctr comes with containerD
    - Not very user friendly
    - Only supports limited features
- ctr tool is soley made for debugging containerD. The nerdctl provides stable and human-friendly user experience.

<img src=../images/02.png width="700"/>

- CLI - nertctl
    - nerdctl provides a Docker like CLI for containerD
    - nerdctl supports docker compose
    - nerdctl supports newest features in containerD
        - Encrypted container images
        - Lazy pulling
        - P2P image distribution
        - Image signing and verifying
        - Namespace in Kubernetes

<img src=../images/03.png width="700"/>

- CLI - crictl
    - crictl provides CLI for CRI compatible container runtimes
    - Installed separately
    - Used to inspect and debug container runtimes and not to create containers ideally
    - Works across different runtimes

<img src=../images/04.png width="700"/>

- docker cli vs crictl

<img src=../images/05.png width="700"/>

<img src=../images/06.png width="700"/>

- Before v1.24

<img src=../images/07.png width="700"/>

- From v1.24

<img src=../images/08.png width="700"/>
- Summary

<img src=../images/09.png width="700"/>
