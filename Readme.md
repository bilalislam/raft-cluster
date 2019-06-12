#Motivasyon

##Türkçe
Default olarak 8000 nolu port leader olarak ayaga kalkar.
Ve diğer nodelar master'a join olur . Herhangi bir nedenden dolayı master down olursa
geri kalan 2 node'dan bir master olarak seçilecektir.Bu durumda alert ile birlikte node tamir edilmesi 
durumunda master kimse ona --join flag ile join olması gerekecektir.
Bu durum da otomatikleştirilebilir.

Asıl konu ise bu raft projesi redis sentinel mode'a alternatif olarak çalışması planlandığından redis master-slave failover 
yönetimini raft ile üzerine alması beklenmektedir.

Yani master redis node'u down oldugunda slave promote edilmesi lazım. Ama redis gibi bir sistem yazılımlarının
port edildiği dil seviyesinde de  yönetilmesi beklenmektedir.

Yani sentinel master alias'ı ile liderin kim oldugunu nasıl buluyorsa,redis bookslave veya servicestack yazılımı client'a tanımlanadan redis master-slave
iplerinden hangisinin master veya slave oludugunu bilmeli ve master write işlemlerini kabul ederken
slave sadece data sync için çalışmalı . Master down oldugunda raft algoritması slave'i master state'ine promote ettiğinden
client master alias'ına gore artık write işlemleri yönlendirmeli ve diger node tamir edilene kadar client tarafından kullanılmamalıdır.


Özetle redis için dogru master-slave konfigurasyonu tanımlandığında client bu ipleri bilmeli ve hiç değişmemelidir.
Redis cluster'ından cevap alınamadığında problem olabileceğinden aslında yine cluster tek bir endpoint üzerinden kendi içinde
master-slave state'lerine sahip sunucu barındırılması düşünülebilir. Yani cluster sadece aynı state'de commodity hardware'lerden değilde 
master-slave state'lerine sahip bir clusterdan oluşabilir ve bu cluster bir endpoint üzerinden temsil edilebilir.

##Nasıl çalışır ?
```sh
$ sh run-node.sh 1 
$ sh run-node.sh 2
$ sh run-node.sh 3
```

#Motivation

##English
By default, 8000 is the port leader.
And the other nodes join the master. If the master is down for any reason
the remaining 2 nodes will be selected as a master.
In case the master will have to join him with the --join flag.
This can also be automated.

The main issue is that this raft project is planned to work as an alternative to redis sentinel mode and redis master-slave failover
management is expected to take over with the raft.

So when the master redis node is down, the slave needs to be promoted. But a system like redis
it is also expected to be managed at the language level in which it is ported.

In other words, with the sentinel master alias, redis bookslave or servicestack software can be defined to the client without redis master-slave.
Know which rope is a master or slave and accept master write operations
The slave should only work for data sync. Raft algorithm promotes slave to master state when master is down
According to the client master alias, write operations should now redirect and should not be used by the client until the other node is repaired.


In summary, when the correct master-slave configuration is defined for redis, the client must know these threads and should not change at all.
In fact, the cluster is still in a single endpoint because there may be problems when no response is received from the Redis cluster.
Then server hosting with master-slave states can be considered. So the cluster is not just commodity hardware in the same state
It can consist of a cluster with master-slave states and can be represented through an endpoint.

##How it works ?
```sh
$ sh run-node.sh 1 
$ sh run-node.sh 2
$ sh run-node.sh 3
```   