$ kubectl get pods
NAME                                      READY   STATUS    RESTARTS   AGE
kafka-broker1-587cdc764-7gzhg             1/1     Running   0          4m15s
kafka-test-client                         1/1     Running   0          4m14s
zookeeper-deployment-1-7dfbcdb969-wkrgb   1/1     Running   0          4m18s

$ kubectl get svc
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
kafka-service   LoadBalancer   10.103.142.48   10.236.1.1    9092:32355/TCP               4m58s
kubernetes      ClusterIP      10.96.0.1       <none>        443/TCP                      24m
zoo1            ClusterIP      10.109.89.20    <none>        2181/TCP,2888/TCP,3888/TCP   4m59s

$ kubectl exec -it kafka-test-client -- /usr/bin/kafka-console-producer --broker-list 10.236.1.1:9092 --topic topic1
>Hello
[2018-12-05 06:44:19,531] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 1 : {topic1=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
[2018-12-05 06:44:19,639] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 3 : {topic1=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
>World
>

$ kubectl exec -it kafka-test-client -- /usr/bin/kafka-console-consumer --bootstrap-server 10.236.1.1:9092 --topic topic1 --from-beginning
Hello
World
