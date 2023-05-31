---
layout: post
categories: Concurrency
tag: [] 
date: 2022-05-30
---





# High Concurrency

![image-20230530103719241](https://p.ipic.vip/oeedft.png)



## High Concurrency

- System design faces the ultimate challenge of handling high-concurrency traffic.
- System is divided into four layers, with the top layer experiencing the highest pressure and the bottom layer having the weakest capacity.
- System architecture should be hierarchical, with flow control implemented at each level.

### Service Gateway Layer:

- Acts as the entry point for all user traffic and handles load balancing to distribute the load across multiple physical nodes.
- Implements user authentication and rate limiting to filter out invalid traffic.
- Includes security measures to protect against DDoS attacks.

### Business Layer:

- Provides various business functionalities to users.
- Implements "separation of concerns" or "frontend-backend separation" to improve user interface rendering and user experience through page caching.
- Receives user requests and delegates the relevant operations to backend service layer.
- Uses asynchronous design and message queues to handle user requests, reducing the pressure on the service layer.
- Implements circuit breaker mechanisms and service degradation to prevent system cascading failures.

### Service Layer:

- Handles business operations and database interactions.
- Deploys on multiple nodes using cloud-based deployment for horizontal scaling to handle complex business operations.
- Utilizes elastic scaling to automatically adjust the number of nodes based on the workload, reducing operational costs.
- Implements service degradation by returning fallback data instead of querying the database under high pressure situations.
- Uses distributed caching to alleviate pressure on the database.

### Database Layer:

- Implements read-write separation by separating production and query databases for optimized performance.
- Implements sharding to distribute the write workload across multiple databases and mitigate disk I/O bottlenecks.
- Utilizes NoSQL databases and big data platforms for data analysis and optimization.



**高併發**

在面對億級流盤時，系統設計的每一個環節都是一個終極考驗，必預做到極致，我們通過分層將系統劃分成了四個

層次，然而這時我們發現，上層抗壓能力強，底層抗壓能力弱，形成了一個倒三角的態勢。因此，對於系統整體架

構來說，應當做到系統分層、逐級限流



在這四個層次中，最上層的服務網關，系統壓力是最大的，所有用戶壓力都要經過它。這時，服務網關首先要做的

是負載均衡，將所有壓力均勻地分布到許多物理節點上來共同扛佳壓力。然而，服務網關不能將所有的流量直接轉

發給下游，它常要通過限流，將最終有效的流量轉發給下游。因此，它常要用戶身份鑒權，陽止不合法的用戶流

量；還需要有限流措施，當業務流量超過系統設計能力時，將過載的那部分流量拒絕掉，以保護下游的穩定運行；

還需要有安全防護，保護系統免受DDos等互聯網攻擊。



接著就到了業務層，面向的是為用戶提供的各項業務功能。這個層次要承擔用戶界面繪制，展現UI界面，因此需

要"動靜分離"或"前後端分離〞，通過頁面緩存更加流暢地展現用戶界面，提高用戶體驗。同時，當用戶在界面中進

行各種操作時，由它來接收用戶請求，但不由它完成相關操作，而是調用後台服務層的服務去完成。所以，前端業

務層的抗壓能力是比較強的，而後端服務層的抗壓能力比較弱，因為它們除了完成各種業務操作，還要讀寫數據

庫。因此，業務層通過異步化設計，先受理用戶請求，然後發送給消息隊列。這樣的設計，既可以讓業務層獲得更

大的吞吐量，又可以降低服務層的壓力，讓服務層能從容地完成各項業務，當下游的服務層快扛不住流量壓力而大

量超時的時候，業務層通過熔斷機制及時進行」服務降級」來防止系統雪崩。



下游的服務層，除了要完成各種業務操作以外，還需要讀寫數據庫，因此讀寫數據庫成了它們最大的瓶頸。為了扛

住複雜業務給服務層帶來的系統壓力，服務層通過雲端部署，將業務分散於更多的節點中進行橫向擴容，從而扛街

業務壓力。通過雲平台的彈性可伸縮，當壓力大時自動擴展到更多節點，而當壓力小時自動收縮，就能很好地應對

互聯網壓力的彈性變化，從而降低系統的運營成本。



此外，服務層的最大瓶頸是對數據庫的讀寫。當系統壓力過大、數據庫扛不住時，服務層就會啓動服務降級。查詢

的服務降級，就是通過不查詢數據而返回兜底數據來降低數據庫壓力；寫操作的服務降級，就是不再去寫數據庫，

而是切換為寫Redis內存數據庫加異步寫庫，從而扛住系統壓力。此外，分布式緩存也是服務層降低數據庫壓力的

有效措施之一。



最後一層是抗壓能力最差的數據庫。通過讀寫分離將數據庫分為生產庫與查詢庫，分別予以系統優化；通過橫向、

縱向的數據分庫分散生產庫的寫入壓力，緩解磁盤1/0的瓶頸；通過NoSQL數據庫與大數據平台實現數據分析與查

詢的優化。這些都是解決數據層系統壓力、提升吞吐量的有效措施。

一主多從

表分庫抵抗寫的壓力，TIDB

TDB是一個開源的NewSQL資料庫，可作為mysq的從服務，作橫向分庫分表





## High Availability

- System design should be able to tolerate single point failures.
- Implement multiple data centers and utilize DNS round-robin for gateway access to ensure availability even if one data center goes down.
- Service gateways should achieve high availability through load balancing (e.g. Nginx master-slave synchronization) and distribution across multiple gateways.
- Application and service layers should deploy on multiple nodes using Kubernetes for cloud-based deployment.
- Failed nodes should be replaced, and unfinished tasks should be transferred to other nodes through failover to maintain high availability.
- Data nodes (caches, message queues, databases) should utilize master-slave replication for high availability.
- 

## High Reliability

- Ensure non-loss operation of data in high-concurrency systems.
- Strengthen design to prevent data loss during automatic master-slave switching moments.
- Move towards decentralized architectures to eliminate the distinction between master and slave nodes and enable data replication across multiple nodes.
- Increase capacity or allow dynamic resource allocation to improve reliability.







**高可用**

高可用要求，即使在面對高井發時個別節點宕機，整個系統對於用戶來說仍是可用的，用戶的所有請求都將予以逃

理，並最終反饋給用戶。在系統面對高併發時，任何一個節點任何時候都可能出現宕機。因此系統設計應當具備

"單點故障可容忍"的特性，並將該特性體現在系統設計的每一個環節。

首先，網關層在面對互聯網的時候，可能因為網絡故障而造成整個機房不可用。因此，系統建設必須是多機房，並

且通過DNS的輪詢實現多機房的訪問。這樣，即使一個機房出了問題，還有另一個機房可用。接著，服務網關也

要實現高可用，即首先保證負載均街的高可用（如Nginx的主從同步），然後負載到多個服務網關。這樣，即使一

個股務網關不可用，還有其他服務網關，系統依然保持高可用。



然後是應用層與服務層的高可用。通過Kubernetes雲端部香，每個服務都至少部零在兩個以上節點。如果系統運

行過程中一個節點不可用，那麼就在另一個地方再啓動一個節點。失效的那個節點沒有完成的任務，通過故障轉移

交給另一個節點，雖然會增加一點延遲，但任務最終會完成並返回給用戶，系統還是高可用的。

最後是數據節點，包括緩存、消息隊列、數據庫。這些節點的高可用主要是通過主從同步來實現的，當主節點失效

以後就會自動切換到從節點，將從節點升級為主節點，就能保證高可用。



**高可靠**

這裡的高可靠，特指數據的高可菲運行不丟失，這對於億級高併發系統來說也是非常重要的。在面對高併發壓力

時，個別節點宕機時常發生。但是，節點宕機而自動進行主從切換的瞬間，容易造成數據的丟失。因此，就需要通

過加強設計保證數據的可靠。問題多發生於主從切換，所以未來會越來越多地朝若「去中心化」"的設計發展，即未來

的集群不再有主從之分，或者互為主從，我備份你的數據的同時，你也在備份我的數據，實現數據的多節點複製。

有了這樣的機制，集群中即使某個節點失效，數據也不會丟失，就能更好地保障數據的高可靠運營。

 

可以多買capacity、或需要時，讓系統自己分配



# **Second Kill**

前端靜態、cache、

讀多寫少時別去db查，因為db的連接資源有限，用cache去擋

**cache iussue**

在redis放info，但這裡是不完全可靠的，用戶是要傳商品id，在redis裡被查

**cache** **擊穿**

後來又造成db掛、加鎖



**cache****預熱**

**cache****穿透**

加鎖的處理性能不好呀! 有無更好的解決方案？ --> 布隆過濾器先查商品id看有沒有存在；但redis如果一更新，要跟著更新，這是個新議題



![image-20230530110803915](https://p.ipic.vip/nzekgc.png)



## Redis分布式锁





Ref: 

[亿级流量架构设计-高可用，高可靠_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1fk4y1p7na/?spm_id_from=333.999.0.0&vd_source=a446d08c42a016c121a1c8007fc3ce42)

[阿里三面：秒杀系统如何设计？竟然败在这题了。。。_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1dP411w7Wt/?spm_id_from=333.788&vd_source=a446d08c42a016c121a1c8007fc3ce42)