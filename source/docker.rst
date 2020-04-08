**********************
对比docker
**********************

docker engine
=================================

截至2020年3月28日10:16:24， docker官方支持多种操作系统和芯片架构 [#docker_install]_ , Kunpeng属于aarch64,是支持的

.. csv-table::
   :header: Platform, x86_64 / amd64, ARM, ARM64 / AARCH64, IBM Power (ppc64le), IBM Z（s390x）

   CentOS,  支持,       ,   支持,   ,
   Debian,  支持,   支持,    支持,  ,
   Fedora,  支持,       ,   支持,   ,
   Ubuntu,  支持,   支持,   支持, 支持, 支持


docker swarm
=================================

支持intel和kunpeng混合组网，但是分发任务的时候

.. code-block:: console

    [user1@intel6248 ~]$ docker node ls
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
    4nj18pipvg0rg4879psiql8xe *   intel6248           Ready               Active              Leader              19.03.7
    jle3s6my1znz1yg9z4o450kkp     kunpeng916          Ready               Active                                  19.03.8
    k7cndxruwpyjcauxxdlc83b3t     kunpeng920          Ready               Active                                  19.03.8
    [user1@intel6248 ~]$


.. [#docker_install] https://docs.docker.com/install/