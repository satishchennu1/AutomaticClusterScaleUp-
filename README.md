# AutomaticClusterScaleUp-
Manual Cluster Scale Up/Down 

a) go to the compute --> click on Machine Sets 
b) you should be in openshift-machine-api project 
c) click on the ocppilot-r2vwz-worker-us-east-1a 
d) you can change the Desired Count on the  machine configuration.
e) In the Actions pull down menu (on the right hand side), select Edit Count
   Enter 3 and click Save
f) you will see the current count  and desired count same on all the machines. 
g)    [root@ip-10-201-26-228 ~]# oc get machinesets -n openshift-machine-api
        NAME                               DESIRED   CURRENT   READY   AVAILABLE   AGE
        ocppilot-r2vwz-worker-us-east-1a   2         2         2       2           134d
        ocppilot-r2vwz-worker-us-east-1b   0         0                             134d
        ocppilot-r2vwz-worker-us-east-1c   2         2         2       2           134d
        ocppilot-r2vwz-worker-us-east-1d   1         1         1       1           134d
        ocppilot-r2vwz-worker-us-east-1e   1         1         1       1           134d
        ocppilot-r2vwz-worker-us-east-1f   0         0                             134d
h)  [root@ip-10-201-26-228 ~]# oc get nodes
        NAME                           STATUS   ROLES    AGE     VERSION
        ip-10-0-128-142.ec2.internal   Ready    worker   12d     v1.14.6+9fb2d5cf9
        ip-10-0-130-11.ec2.internal    Ready    worker   134d    v1.14.6+9fb2d5cf9
        ip-10-0-136-148.ec2.internal   Ready    worker   11d     v1.14.6+9fb2d5cf9
        ip-10-0-137-136.ec2.internal   Ready    master   134d    v1.14.6+9fb2d5cf9
        ip-10-0-164-245.ec2.internal   Ready    master   134d    v1.14.6+9fb2d5cf9
        ip-10-0-167-53.ec2.internal    Ready    worker   134d    v1.14.6+9fb2d5cf9
        ip-10-0-174-197.ec2.internal   Ready    worker   7m40s   v1.14.6+9fb2d5cf9
        ip-10-0-190-226.ec2.internal   Ready    master   20d     v1.14.6+9fb2d5cf9
        ip-10-0-190-7.ec2.internal     Ready    worker   11d     v1.14.6+9fb2d5cf9
        ip-10-0-191-131.ec2.internal   Ready    worker   12d     v1.14.6+9fb2d5cf9
        ip-10-0-201-183.ec2.internal   Ready    worker   12d     v1.14.6+9fb2d5cf9
        [root@ip-10-201-26-228 ~]# oc get nodes
        NAME                           STATUS   ROLES    AGE     VERSION
        ip-10-0-128-142.ec2.internal   Ready    worker   12d     v1.14.6+9fb2d5cf9
        ip-10-0-130-11.ec2.internal    Ready    worker   134d    v1.14.6+9fb2d5cf9
        ip-10-0-136-148.ec2.internal   Ready    worker   11d     v1.14.6+9fb2d5cf9
        ip-10-0-137-136.ec2.internal   Ready    master   134d    v1.14.6+9fb2d5cf9
        ip-10-0-164-245.ec2.internal   Ready    master   134d    v1.14.6+9fb2d5cf9
        ip-10-0-167-53.ec2.internal    Ready    worker   134d    v1.14.6+9fb2d5cf9
        ip-10-0-174-197.ec2.internal   Ready    worker   7m40s   v1.14.6+9fb2d5cf9
        ip-10-0-190-226.ec2.internal   Ready    master   20d     v1.14.6+9fb2d5cf9
        ip-10-0-190-7.ec2.internal     Ready    worker   11d     v1.14.6+9fb2d5cf9
        ip-10-0-191-131.ec2.internal   Ready    worker   12d     v1.14.6+9fb2d5cf9
        ip-10-0-201-183.ec2.internal   Ready    worker   12d     v1.14.6+9fb2d5cf9

Automatic Cluster Scale up 

OpenShift can automatically scale the infrastructure based on workload provided  

Current Configuration 

a) oc get machinesets -n openshift-machine-api

b) Define MachineAutoScaler 

   i) In GitHub there is MachineAutoScaler.yaml 

   ii) configurtion is mached according the cluster name and high availability zones . 

   iii) also increased the min and max of the replicas . 

c) oc create -f machine-autoscale-example.yaml -n openshift-machine-api 

d) [root@ip-10-201-26-228 MachineAutoScaler]# oc create -f machineautoscaler.yaml -n openshift-machine-api
            machineautoscaler.autoscaling.openshift.io/autoscale-us-east-1a-q2shz created
            machineautoscaler.autoscaling.openshift.io/autoscale-us-east-1b-gl785 created
            machineautoscaler.autoscaling.openshift.io/autoscale-us-east-1c-8gnxw created
            machineautoscaler.autoscaling.openshift.io/autoscale-us-east-1d-vncp7 created
            machineautoscaler.autoscaling.openshift.io/autoscale-us-east-1e-72gm7 created
            machineautoscaler.autoscaling.openshift.io/autoscale-us-east-1f-mbnk5 created

e) create AutoScaler command 

   i) wget https://raw.githubusercontent.com/RedHatWorkshops/openshiftv4-workshop/master/solutions/cluster-autoscaler.yaml

   ii) change the MaxTotal nodes in the  yaml 

   iii) [root@ip-10-201-26-228 MachineAutoScaler]# oc create -f cluster-autoscaler.yaml
              clusterautoscaler.autoscaling.openshift.io/default created
f)  here is the output for the autoscaler 
      go to compute --> Machine Autoscalers --> see the configuration 

Defined job 

https://raw.githubusercontent.com/openshift/training/master/assets/job-work-queue.yaml

it will produce massive load that cluster cannot  

a) oc adm new-project autoscale-example && oc project autoscale-example 

b) go to monitoring --> dashboards 

c) log in with htpasswd 

d) allow selected permissions  

f) you will see Grafana Homepage , Grafana is configured to use an openshift  user 

e) Home --> Kubernates --> 

https://grafana-openshift-monitoring.apps.ocppilot.ocpcontainer.com/d/efa86fd1d0c121a26444b636a3f509a8/kubernetes-compute-resources-cluster?orgId=1&refresh=10s

force autoscaling event 

$ oc create -n autoscale-example -f https://raw.githubusercontent.com/openshift/training/master/assets/job-work-queue.yaml


[root@ip-10-201-26-228 MachineAutoScaler]# oc get pod -n autoscale-example
NAME                     READY   STATUS              RESTARTS   AGE
work-queue-8hzkq-2dwlc   0/1     Pending             0          8s
work-queue-8hzkq-4fqkf   0/1     Pending             0          8s
work-queue-8hzkq-4mqks   0/1     Pending             0          7s
work-queue-8hzkq-5bt6l   0/1     Pending             0          8s
work-queue-8hzkq-5qghg   0/1     Pending             0          8s
work-queue-8hzkq-5wl84   0/1     Pending             0          8s
work-queue-8hzkq-5xfn6   0/1     Pending             0          7s
work-queue-8hzkq-62mr9   0/1     Pending             0          7s
work-queue-8hzkq-6fkrr   0/1     Pending             0          7s
work-queue-8hzkq-7kfg9   0/1     Pending             0          7s
work-queue-8hzkq-8fz2t   0/1     Pending             0          8s
work-queue-8hzkq-8g5ws   0/1     Pending             0          8s
work-queue-8hzkq-8wldl   0/1     ContainerCreating   0          8s
work-queue-8hzkq-9nsr5   0/1     Pending             0          8s
work-queue-8hzkq-bkshr   0/1     Pending             0          7s
work-queue-8hzkq-bnd44   0/1     Pending             0          8s
work-queue-8hzkq-bptzs   0/1     Pending             0          8s
work-queue-8hzkq-bsw64   0/1     ContainerCreating   0          8s
work-queue-8hzkq-bxjj6   0/1     Pending             0          8s
work-queue-8hzkq-c4jwz   0/1     Pending             0          7s
work-queue-8hzkq-chz7c   0/1     Pending             0          8s
work-queue-8hzkq-ddv7k   0/1     Pending             0          7s
work-queue-8hzkq-dv8qv   0/1     Pending             0          8s
work-queue-8hzkq-fq7cv   0/1     ContainerCreating   0          8s
work-queue-8hzkq-hztp4   0/1     Pending             0          8s
work-queue-8hzkq-jbdgq   0/1     Pending             0          7s
work-queue-8hzkq-jfp2c   0/1     Pending             0          8s
work-queue-8hzkq-k9868   0/1     ContainerCreating   0          8s
work-queue-8hzkq-klhht   0/1     Pending             0          8s
work-queue-8hzkq-l4hpf   0/1     Pending             0          7s
work-queue-8hzkq-lkgfd   0/1     Pending             0          8s
work-queue-8hzkq-mr2lq   0/1     Pending             0          8s
work-queue-8hzkq-nq65x   0/1     Pending             0          8s
work-queue-8hzkq-nqp7k   0/1     ContainerCreating   0          8s
work-queue-8hzkq-p9lg4   0/1     Pending             0          8s
work-queue-8hzkq-pvk58   0/1     Pending             0          7s
work-queue-8hzkq-q6z92   0/1     Pending             0          7s
work-queue-8hzkq-qn7ld   0/1     Pending             0          7s
work-queue-8hzkq-r56wx   0/1     Pending             0          8s
work-queue-8hzkq-rhl2l   0/1     Pending             0          7s
work-queue-8hzkq-s628g   0/1     Pending             0          7s
work-queue-8hzkq-sj5qj   0/1     Pending             0          8s
work-queue-8hzkq-tcdld   0/1     Pending             0          7s
work-queue-8hzkq-ttlg2   0/1     Pending             0          8s
work-queue-8hzkq-v6crx   0/1     ContainerCreating   0          8s
work-queue-8hzkq-vdkzf   0/1     Pending             0          7s
work-queue-8hzkq-w2fjp   0/1     Pending             0          7s
work-queue-8hzkq-x2f67   0/1     Pending             0          8s
work-queue-8hzkq-xlkbb   0/1     Pending             0          7s
work-queue-8hzkq-z7r95   0/1     Pending             0          8s


[root@ip-10-201-26-228 MachineAutoScaler]# oc get machines -n openshift-machine-api
NAME                                     STATE     TYPE        REGION      ZONE         AGE
ocppilot-r2vwz-master-0                  running   m4.xlarge   us-east-1   us-east-1a   134d
ocppilot-r2vwz-master-1                  running   m4.xlarge   us-east-1   us-east-1d   20d
ocppilot-r2vwz-master-2                  running   m4.xlarge   us-east-1   us-east-1c   134d
ocppilot-r2vwz-worker-us-east-1a-7snr7   running   m4.large    us-east-1   us-east-1a   34m
ocppilot-r2vwz-worker-us-east-1a-f9kdd   pending   m4.large    us-east-1   us-east-1a   4s
ocppilot-r2vwz-worker-us-east-1a-hhdfl   running   m4.large    us-east-1   us-east-1a   134d
ocppilot-r2vwz-worker-us-east-1a-rsw82                                                  4s
ocppilot-r2vwz-worker-us-east-1a-z4p8h                                                  4s
ocppilot-r2vwz-worker-us-east-1c-7z7kh   pending   m4.large    us-east-1   us-east-1c   14s
ocppilot-r2vwz-worker-us-east-1c-bjg8c   pending   m4.large    us-east-1   us-east-1c   14s
ocppilot-r2vwz-worker-us-east-1c-fc9ng   running   m4.large    us-east-1   us-east-1c   134d
ocppilot-r2vwz-worker-us-east-1c-k2b8v   pending   m4.large    us-east-1   us-east-1c   14s
ocppilot-r2vwz-worker-us-east-1c-zlqs5   pending   m4.large    us-east-1   us-east-1c   14s
ocppilot-r2vwz-worker-us-east-1d-qgzvc   running   m4.large    us-east-1   us-east-1d   12d
ocppilot-r2vwz-worker-us-east-1e-nnhqz   running   m4.large    us-east-1   us-east-1e   12d


[root@ip-10-201-26-228 MachineAutoScaler]# oc get machines -n openshift-machine-api
NAME                                     STATE     TYPE        REGION      ZONE         AGE
ocppilot-r2vwz-master-0                  running   m4.xlarge   us-east-1   us-east-1a   134d
ocppilot-r2vwz-master-1                  running   m4.xlarge   us-east-1   us-east-1d   20d
ocppilot-r2vwz-master-2                  running   m4.xlarge   us-east-1   us-east-1c   134d
ocppilot-r2vwz-worker-us-east-1a-7snr7   running   m4.large    us-east-1   us-east-1a   35m
ocppilot-r2vwz-worker-us-east-1a-f9kdd   running   m4.large    us-east-1   us-east-1a   74s
ocppilot-r2vwz-worker-us-east-1a-hhdfl   running   m4.large    us-east-1   us-east-1a   134d
ocppilot-r2vwz-worker-us-east-1a-rsw82   running   m4.large    us-east-1   us-east-1a   74s
ocppilot-r2vwz-worker-us-east-1a-z4p8h   running   m4.large    us-east-1   us-east-1a   74s
ocppilot-r2vwz-worker-us-east-1c-7z7kh   running   m4.large    us-east-1   us-east-1c   84s
ocppilot-r2vwz-worker-us-east-1c-bjg8c   running   m4.large    us-east-1   us-east-1c   84s
ocppilot-r2vwz-worker-us-east-1c-fc9ng   running   m4.large    us-east-1   us-east-1c   134d
ocppilot-r2vwz-worker-us-east-1c-k2b8v   running   m4.large    us-east-1   us-east-1c   84s
ocppilot-r2vwz-worker-us-east-1c-zlqs5   running   m4.large    us-east-1   us-east-1c   84s
ocppilot-r2vwz-worker-us-east-1d-5p75k   running   m4.large    us-east-1   us-east-1d   64s
ocppilot-r2vwz-worker-us-east-1d-cfxcs   running   m4.large    us-east-1   us-east-1d   64s
ocppilot-r2vwz-worker-us-east-1d-qgzvc   running   m4.large    us-east-1   us-east-1d   12d
ocppilot-r2vwz-worker-us-east-1d-r92wg   running   m4.large    us-east-1   us-east-1d   64s
ocppilot-r2vwz-worker-us-east-1d-rgsch   running   m4.large    us-east-1   us-east-1d   64s
ocppilot-r2vwz-worker-us-east-1e-ctn99   running   m4.large    us-east-1   us-east-1e   54s
ocppilot-r2vwz-worker-us-east-1e-nnhqz   running   m4.large    us-east-1   us-east-1e   12d


