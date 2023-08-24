---
title: EFK
date: 2023-08-24 16:34:28
categories:
  - [kubernetes]
tags: logging
---
要在Kubernetes中部署Filebeat、Elasticsearch和Kibana来采集容器日志，可以按照以下步骤进行:

1. 部署Elasticsearch:
   在Kubernetes集群上创建一个Elasticsearch的Deployment，这个Deployment将用于存储和索引日志数据。可以使用以下示例配置文件创建Deployment:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: elasticsearch
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: elasticsearch
     template:
       metadata:
         labels:
           app: elasticsearch
       spec:
         containers:
         - name: elasticsearch
           image: elasticsearch:7.12.1
           resources:
             requests:
               memory: 2Gi
               cpu: 1
           ports:
           - containerPort: 9200
   ```

   运行`kubectl apply -f elasticsearch.yaml`命令来创建Deployment。

2. 部署Kibana:
   在Kubernetes集群上创建一个Kibana的Deployment，这个Deployment将用于可视化和查询日志数据。可以使用以下示例配置文件创建Deployment:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: kibana
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: kibana
     template:
       metadata:
         labels:
           app: kibana
       spec:
         containers:
         - name: kibana
           image: kibana:7.12.1
           resources:
             requests:
               memory: 1Gi
               cpu: 0.5
           ports:
           - containerPort: 5601
   ```

   运行`kubectl apply -f kibana.yaml`命令来创建Deployment。

3. 部署Filebeat:
   在Kubernetes集群上创建一个Filebeat DaemonSet，这个DaemonSet将在每个节点上运行一个Filebeat实例来收集容器日志。可以使用以下示例配置文件创建DaemonSet:

   ```yaml
   apiVersion: apps/v1
   kind: DaemonSet
   metadata:
     name: filebeat
     labels:
       app: filebeat
   spec:
     selector:
       matchLabels:
         app: filebeat
     template:
       metadata:
         labels:
           app: filebeat
       spec:
         containers:
         - name: filebeat
           image: docker.elastic.co/beats/filebeat:7.12.1
           volumeMounts:
           - name: varlibdockercontainers
             mountPath: /var/lib/docker/containers
             readOnly: true
           - name: varlog
             mountPath: /var/log
             readOnly: true
           - name: varlogpod
             mountPath: /var/log/pods
             readOnly: true
           - name: varrun
             mountPath: /var/run
           - name: varlibkubelet
             mountPath: /var/lib/kubelet
           - name: varlognode
             mountPath: /var/log/node
             readOnly: true
           - name: varlogcontainersnew
             mountPath: /var/log/containersnew
             readOnly: true
           - name: dockersocket
             mountPath: /var/run/docker.sock
           - name: config
             mountPath: /usr/share/filebeat/filebeat.yml
             subPath: filebeat.yml
             readOnly: true
           env:
           - name: K8S_NODE_NAME
             valueFrom:
               fieldRef:
                 fieldPath: spec.nodeName
           - name: NODE_NAME
             valueFrom:
               fieldRef:
                 fieldPath: spec.nodeName
           - name: FILEBEAT_HOST
             valueFrom:
               fieldRef:
                 fieldPath: status.hostIP
           - name: FILEBEAT_CONFIG_CHECK_FREQUENCY
             value: "5s"
           - name: OUTPUT_TYPE
             value: elasticsearch
           - name: ELASTICSEARCH_HOST
             value: elasticsearch:9200
           - name: ELASTICSEARCH_USERNAME
             value: "<ELASTICSEARCH_USERNAME>"
           - name: ELASTICSEARCH_PASSWORD
             value: "<ELASTICSEARCH_PASSWORD>"
           - name: ELASTICSEARCH_INDEX
             value: "filebeat-%{[agent.version]}-%{+yyyy.MM.dd}"
           - name: ELASTICSEARCH_PIPELINE
             value: "<ELASTICSEARCH_PIPELINE>"
           resources:
             limits:
               memory: 200Mi
             requests:
               cpu: 100m
               memory: 100Mi
           terminationGracePeriodSeconds: 30
           dnsPolicy: ClusterFirstWithHostNet
           restartPolicy: Always
           hostNetwork: true
           volumes:
           - name: varlibdockercontainers
             hostPath:
               path: /var/lib/docker/containers
           - name: varlog
             hostPath:
               path: /var/log
           - name: varlogpod
             hostPath:
               path: /var/log/pods
           - name: varrun
             hostPath:
               path: /var/run
           - name: varlibkubelet
             hostPath:
               path: /var/lib/kubelet
           - name: varlognode
             hostPath:
               path: /var/log/node
           - name: varlogcontainersnew
             hostPath:
               path: /var/log/containersnew
           - name: dockersocket
             hostPath:
               path: /var/run/docker.sock
           - name: config
             configMap:
               name: filebeat-config
       updateStrategy:
         type: RollingUpdate
        rollingUpdate:
           maxUnavailable: 1
   ```

   创建一个ConfigMap来存储Filebeat的配置文件(filebeat.yml)，示例配置文件可以参考以下内容:

   ```yaml
   filebeat.config:
     modules:
       path: ${path.config}/modules.d/*.yml
       reload.enabled: false

   filebeat.modules:
   - module: system
     syslog:
       enabled: true
     auth:
       enabled: true
     package:
       enabled: true
     coredns:
       enabled: true

   output.elasticsearch:
     hosts: ['elasticsearch:9200']
     username: '<ELASTICSEARCH_USERNAME>'
     password: '<ELASTICSEARCH_PASSWORD>'
     pipeline: '<ELASTICSEARCH_PIPELINE>'
   ```

   运行`kubectl create configmap filebeat-config --from-file=filebeat.yml`命令来创建ConfigMap。

   然后，运行`kubectl apply -f filebeat.yaml`命令来创建DaemonSet。

4. 日志采集:
   当Filebeat启动后，它将开始采集Kubernetes集群中所有容器的日志数据，并将其发送到Elasticsearch进行存储和索引。您可以使用Kibana来可视化和查询这些日志数据。

这是一个简单的部署Filebeat、Elasticsearch和Kibana来采集Kubernetes集群中容器日志的示例配置。根据您的实际需求，可能还需要进行一些其他的配置和调整。