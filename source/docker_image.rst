*********************************
容器镜像架构差异 docker image
*********************************

大多数时候，使用docker是没有差异的， 但是如果image不支持multi-arch [#multi_arch]_ ，docker run会出错

这里以Kubernetes官方的小集群工具minikube介绍的echoserver [#echoserver]_ 为例。

在X86上运行，可以正常启动 ::

    user1@intel6248:~$ docker run --rm --name echoserver -p 8080:80 -p 1443:443 k8s.gcr.io/echoserver:1.4

    user1@intel6248:~$ docker ps
    CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS              PORTS                                                                           NAMES
    099015e52159        k8s.gcr.io/echoserver:1.4            "nginx -g 'daemon of…"   7 minutes ago       Up 7 minutes        443/tcp, 0.0.0.0:11080->80/tcp


在ARM上运行，会出错 ::

    user1@Arm64-server:~$  docker run --rm --name echoserver -p 8080:80 -p 1443:443 k8s.gcr.io/echoserver:1.4
    standard_init_linux.go:211: exec user process caused "exec format error"


可以查看拉到本地的镜像， 当前只支持ARM64  ::

    user1@Arm64-server:~$ docker image inspect k8s.gcr.io/echoserver:1.4 | grep Architecture
            "Architecture": "amd64",

如果镜像不支持ARM64怎么办？
===========================

首先找到echoserver的Dockerfile [#echoserver_dockerfile]_

根据kubernetes中镜像的构建指导 [#kubernetes_images]_ ::

    make all WHAT=agnhost
    make all WHAT=echoserver

可以生成echoserver的多架构镜像 ::

    REPOSITORY                                     TAG                                        IMAGE ID            CREATED             SIZE
    gcr.io/kubernetes-e2e-test-images/echoserver   2.3-linux-ppc64le                          5fb747e030a1        10 hours ago        27.3MB
    gcr.io/kubernetes-e2e-test-images/echoserver   2.3-linux-arm64                            26c05e1e91ff        10 hours ago        22MB
    gcr.io/kubernetes-e2e-test-images/echoserver   2.3-linux-arm                              d68f0795cc5d        10 hours ago        19.7MB
    gcr.io/kubernetes-e2e-test-images/echoserver   2.3-linux-amd64                            c797d5221613        15 hours ago        19.3MB

这个时候的镜像是基于ARM64的 ::

    user1@intel6248:~/kubernetes/test/images$ docker inspect gcr.io/kubernetes-e2e-test-images/echoserver:2.3-linux-arm64 | grep Arch
            "Architecture": "arm64",

.. [#multi_arch] https://docs.docker.com/docker-for-mac/multi-arch/
.. [#echoserver] https://minikube.sigs.k8s.io/docs/start/
.. [#echoserver_dockerfile] https://github.com/kubernetes/kubernetes/blob/master/test/images/echoserver/Dockerfile
.. [#kubernetes_images] https://github.com/kubernetes/kubernetes/tree/master/test/images