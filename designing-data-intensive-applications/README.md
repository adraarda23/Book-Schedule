# Designing Data-Intensive Applications

## Genel Bilgiler
- **Yazar**: Martin Kleppmann
- **Yayın Yılı**: 2017 (O'Reilly Media)
- **Format**: PDF/EPUB
- **Durum**: ✅ Tamamlandı

## Özet
Martin Kleppmann'ın modern veri sistemlerinin tasarım prensiplerini derinlemesine ele aldığı, yazılım mühendisliği alanının en önemli referans kitaplarından biri. Veritabanları, dağıtık sistemler, replication, partitioning, transaction'lar, consensus algoritmaları ve veri yoğun uygulamaların tasarımı gibi kritik konuları akademik titizlikle, fakat pratik örneklerle birlikte sunuyor. "Tek bir doğru çözüm yoktur, sadece trade-off'lar vardır" yaklaşımıyla mühendislik perspektifi kazandırıyor.

## Ana Konular
- [x] **Part I: Foundations of Data Systems** (Bölüm 1-4)
  - Reliable, Scalable, and Maintainable Applications
  - Data Models and Query Languages
  - Storage and Retrieval
  - Encoding and Evolution
- [x] **Part II: Distributed Data** (Bölüm 5-9)
  - Replication
  - Partitioning
  - Transactions
  - The Trouble with Distributed Systems
  - Consistency and Consensus
- [x] **Part III: Derived Data** (Bölüm 10-12)
  - Batch Processing
  - Stream Processing
  - The Future of Data Systems

## Okuma Planı
### Başlangıç Tarihi
Şubat 2026

### Bitiş Tarihi (Hedef)
Mayıs 2026

### Gerçek Bitiş Tarihi
Ağustos 2026 — *Part III (Derived Data) planlanandan geç tamamlandı; batch/stream processing bölümleri beklediğimden yoğun çıktı.*

### İlerleme
- [x] Başlangıç
- [x] %25 tamamlandı
- [x] %50 tamamlandı
- [x] %75 tamamlandı
- [x] ✅ Tamamlandı

## Bölümler ve Notlar

### Part I: Foundations of Data Systems

#### Chapter 1: Reliable, Scalable, and Maintainable Applications
Veri yoğun uygulamaların üç temel kalitesi:
- **Reliability**: Sistemin hata (fault) durumunda bile doğru çalışmaya devam etmesi. Fault ≠ Failure. Netflix'in Chaos Monkey gibi araçlarla hatayı bilinçli tetikleyerek dayanıklılık ölçülür.
- **Scalability**: Yük (load) arttıkça sistemin performansını koruyabilmesi. Twitter örneğinde fan-out yaklaşımı (push vs. pull) güzel bir trade-off örneği.
- **Maintainability**: Operability, simplicity ve evolvability. Sistemin uzun vadede başkaları tarafından geliştirilebilir kalması.

**Önemli Kavramlar**: Latency vs. response time, percentiles (p50, p95, p99), tail latency amplification.

#### Chapter 2: Data Models and Query Languages
- **Relational vs. Document vs. Graph** modelleri karşılaştırması
- Document model: Self-contained, hierarchical veri için iyi (one-to-many ilişkiler), join zayıflığı var
- Relational model: Many-to-many ilişkiler ve join'ler için güçlü
- Graph model: Highly interconnected veri için (sosyal ağlar, knowledge graph)
- **Schema-on-read vs. schema-on-write**: NoSQL'in flexible schema iddiası aslında schema-on-read yaklaşımı
- **Declarative (SQL) vs. Imperative (MapReduce)** sorgu dilleri
- Cypher, SPARQL, Datalog gibi graph query language örnekleri

#### Chapter 3: Storage and Retrieval
Bu bölüm kitabın en aydınlatıcı bölümlerinden:
- **Log-structured storage (LSM-Trees)**: Sequential write, compaction, SSTables (Sorted String Tables). LevelDB, RocksDB, Cassandra'nın temeli.
- **Page-oriented storage (B-Trees)**: Klasik RDBMS standardı. In-place update, WAL (Write-Ahead Log).
- **LSM vs. B-Tree Trade-off**: LSM yazmada daha hızlı, B-Tree okumada genelde daha hızlı. Write amplification kavramı.
- **OLTP vs. OLAP**: Transaction processing ile analytical processing'in farklı veri yapıları gerektirmesi.
- **Column-oriented storage**: Data warehouse'larda satır yerine sütun bazlı depolama. Compression çok daha etkili (bitmap encoding, run-length encoding). Vectorized processing.
- **Materialized views ve data cubes** kavramları.

#### Chapter 4: Encoding and Evolution
- **Serialization formatları**: JSON, XML, CSV (textual); Thrift, Protocol Buffers, Avro (binary).
- **Backward compatibility**: Yeni kod eski veriyi okuyabilmeli.
- **Forward compatibility**: Eski kod yeni veriyi okuyabilmeli.
- **Avro'nun unique özelliği**: Writer schema ve reader schema ayrımı, schema evolution için en esnek format.
- **Veri akış modelleri**: Database üzerinden, RPC ile, asenkron mesajlaşma ile.
- **Rolling upgrade** sürecinde schema evolution çok kritik.

---

### Part II: Distributed Data

#### Chapter 5: Replication
Veriyi çoklu node'larda tutmanın 3 temel yaklaşımı:
- **Single-leader replication**: PostgreSQL, MySQL, MongoDB. Synchronous vs. asynchronous replication. Replication lag sorunları.
- **Multi-leader replication**: Multi-datacenter, offline-capable clients (CouchDB). Write conflict çözümü gerekiyor (LWW, version vectors, CRDT).
- **Leaderless replication**: Dynamo, Cassandra, Riak. Quorum (w + r > n), read repair, anti-entropy, sloppy quorum, hinted handoff.

**Önemli Kavramlar**:
- **Read-your-writes consistency, monotonic reads, consistent prefix reads**
- **Replication lag** problemleri ve çözümleri
- **Conflict resolution**: Vector clocks ve version vectors
- Dynamo-style sistemlerde "eventually consistent" garantilerinin nüansları

#### Chapter 6: Partitioning (Sharding)
- **Partitioning stratejileri**:
  - **Key range partitioning**: Sıralı erişim için iyi ama hotspot riski (örneğin tarih bazlı keys)
  - **Hash-based partitioning**: Yük dengeli ama range query'ler zor
- **Secondary indexes ile partitioning**:
  - **Local index (document-partitioned)**: Yazmada hızlı, okumada scatter/gather
  - **Global index (term-partitioned)**: Okumada hızlı, yazmada cross-partition
- **Rebalancing**:
  - Hash mod N kötü fikir (her node sayısı değişiminde herşey yeniden taşınır)
  - **Consistent hashing** (Dynamo)
  - **Fixed number of partitions** (Riak, Elasticsearch)
  - **Dynamic partitioning** (HBase)
- **Request routing**: Service discovery, ZooKeeper kullanımı

#### Chapter 7: Transactions
Bu bölüm gerçekten paradigma değiştirici:
- **ACID** kavramının kitaplardan farklı yorumları olduğu gerçeği
- **Atomicity**: All or nothing (concurrency ile karıştırılmamalı)
- **Consistency**: Aslında uygulama seviyesinde bir özellik (database'in işi değil)
- **Isolation**: En karmaşık ve yanlış anlaşılan kısım
- **Durability**: fsync, replication

**Isolation Levels (Anomaliler ile birlikte)**:
- **Read Committed**: Dirty read/write yok (en yaygın default)
- **Snapshot Isolation (Repeatable Read)**: MVCC ile gerçeklenir. Non-repeatable read ve read skew'i önler. Long-running read query'ler için ideal.
- **Serializable**: En güçlü. Üç farklı şekilde gerçeklenir:
  - **Actual serial execution** (VoltDB, Redis)
  - **Two-Phase Locking (2PL)**: Predicate locks, index-range locks
  - **Serializable Snapshot Isolation (SSI)**: Optimistic, PostgreSQL'de var

**Yaygın Anomaliler**: Lost update, write skew, phantom reads. Write skew'i sadece serializability çözüyor.

#### Chapter 8: The Trouble with Distributed Systems
Bu bölüm dağıtık sistemlerle ilgili tüm yanılgıları yıkıyor:
- **Unreliable Networks**: Packet kaybı, asenkron network, partition'lar. TCP timeout'ları belirsizdir.
- **Unreliable Clocks**:
  - **Time-of-day clocks** (wall clock) NTP sync yüzünden geriye gidebilir
  - **Monotonic clocks** süre ölçümü için kullanılmalı
  - **Clock skew** dağıtık sistemlerde major problem
  - **Google Spanner'ın TrueTime API'si** ve uncertainty intervals
- **Process pauses**: GC, virtualization, swap, page fault → Lider sandığın node aslında lider olmayabilir
- **Knowledge, Truth, and Lies**: Quorum karar verme, fencing tokens (split-brain önleme)
- **Byzantine faults** ve neden çoğu sistemde göz ardı edildiği

**Önemli Sonuç**: Dağıtık sistemlerde "şu anda" diye birşey yok. Her şey timeout ve probabilistic guarantee'lere dayanıyor.

#### Chapter 9: Consistency and Consensus
- **Linearizability**: Atomic register, "recency guarantee". CAP theorem'in modern yorumu.
- **CAP theorem'in doğru yorumu**: "Either Consistent or Available when Partitioned" — pratikte çok dar bir tanım, abartılıyor.
- **Ordering Guarantees**:
  - **Causal ordering**: Happens-before ilişkisi (Lamport timestamps, vector clocks)
  - **Total ordering**: Single leader doğal olarak verir
  - Causality < Linearizability ama causal genelde yeterli
- **Sequence number generation**: Lamport timestamps + total order broadcast
- **Distributed Transactions**:
  - **Two-Phase Commit (2PC)**: Coordinator failure problemi, blocking
  - **XA transactions** ve neden modern sistemlerde sevilmediği
  - **Three-Phase Commit** ve pratik problemleri
- **Consensus Algorithms**:
  - **Paxos, Raft, Zab, Viewstamped Replication**
  - **FLP impossibility** ve neden randomization/timeout gerektiği
  - **ZooKeeper, etcd**: Linearizable storage, configuration, service discovery, leader election
- **Membership and Coordination Services**: Distributed lock'lar için fencing tokens şart

**Önemli Sonuç**: Consensus problemi atomic broadcast'a eşdeğer. Lider seçimi, dağıtık kilit, atomic commit — hepsi consensus'e dayanıyor.

---

### Part III: Derived Data

Bölümün omurgası **system of record** (source of truth) ile **derived data** (cache, index, materialized view — kaybolursa yeniden üretilebilir) ayrımı.

#### Chapter 10: Batch Processing
- **Unix felsefesi → MapReduce**: Immutable input, ayrı output, composable araçlar. MapReduce'un asıl işi aradaki distributed sort/shuffle.
- **Join stratejileri**: Reduce-side (sort-merge, skew problemi) vs. map-side (broadcast hash join, partitioned hash join).
- **Immutable input'un değeri**: Job'u tekrar çalıştırmak güvenli → "human fault tolerance", hatalı deploy'dan geri dönebilme.
- **Dataflow engine'ler** (Spark, Tez, Flink): Tüm işi tek DAG olarak modelleme, intermediate state'i HDFS'e materialize etmeme, lineage ile recovery.

#### Chapter 11: Stream Processing
- **Broker türleri**: AMQP/JMS tarzı (mesaj consume edilince silinir, redelivery sırayı bozar) vs. log-based (Kafka — offset, replay, sıra garantisi).
- **Dual write problemi** ve çözümü: CDC (WAL/binlog → stream) ve **event sourcing**. Command reddedilebilir, event olmuş bitmiştir.
- **Event time vs. processing time**: Consumer geride kalıp sonra yetiştiğinde processing time'a göre pencereleme tamamen yanlış metrik üretiyor. Watermark, straggler event'ler.
- **Join'ler**: stream-stream (window), stream-table (enrichment), table-table (materialized view). Join'ler zamana bağımlı — aynı sorgu farklı zamanda farklı sonuç verebiliyor.
- **"Exactly-once" aslında effectively-once**: Checkpoint iç state'i kurtarır, dış yan etkiler için idempotence şart.

#### Chapter 12: The Future of Data Systems
- **Data integration**: "En iyi veritabanı" yok; asıl problem RDBMS + index + cache + warehouse'u tutarlı tutmak. Çözüm: total order log'u source of truth yapıp geri kalanı türetmek.
- **Lambda architecture eleştirisi**: Aynı mantığı iki kez yazmak yerine log replay ile batch/stream'i birleştirmek.
- **Unbundling the database**: Storage, index, replication, materialized view'ı sistem seviyesinde ayrıştırıp dataflow ile bağlamak. Uygulama kodu bir "derivation function".
- **End-to-end argument**: Transaction veya "exactly-once" altyapısı uygulama seviyesindeki duplicate submit'i çözmez; doğruluk uçtan uca bir sorumluluk (operation ID + idempotence).
- **Timeliness vs. Integrity**: Timeliness geçici bozulur ve düzelir, **integrity ihlali kalıcı hasardır**. Çoğu iş kolu eventual consistency'yi kaldırır, veri kaybını kaldıramaz.
- **Doing the Right Thing**: Predictive analytics'te önyargı, feedback loop'lar, gözetim ve consent. Teknik bir kitabın etikle bitmesi tesadüf değil.

---

## Notlar

### Önemli Alıntılar
- "There are no easy answers, only trade-offs."
- "Reliability means making systems work correctly, even when faults occur."
- "Consistency is a confusingly overloaded term." (Linearizability vs. ACID consistency vs. consistent hashing)
- "It is better to have unreliable hardware and tolerate failures in software than to make hardware highly reliable."
- "In a distributed system, it doesn't matter what time it 'really' is — what matters is the order of events."
- "Violations of timeliness are 'eventual consistency,' whereas violations of integrity are 'perpetual inconsistency.'"

### Kişisel Çıkarımlar
Bu kitap kariyerimi en çok etkileyen kaynaklardan biri oldu. DDIA'yı okumadan önce veritabanlarına "magic box" gibi bakıyordum — sorgu atılır, sonuç gelir. Şimdi B-tree ile LSM-tree arasındaki write amplification trade-off'unu, MVCC'nin nasıl çalıştığını, neden Postgres'te VACUUM gerekli olduğunu, replication lag'in neden bazı garantileri bozduğunu kavradım.

**En çok değer katan konular**:
1. **Chapter 3 (Storage and Retrieval)**: B-tree ve LSM-tree'nin gerçek mekanikleri. Cassandra ile PostgreSQL'in neden farklı workload'lar için tasarlandığını anlamak.
2. **Chapter 7 (Transactions)**: Isolation level'larının her birinin gerçekten neyi koruyup neyi korumadığı. Write skew gibi sinsi anomaliler.
3. **Chapter 8 (Distributed Systems Troubles)**: Bu bölüm beni distributed systems konusunda sağlıklı bir paranoyaya soktu. Network, clock, process pause — hiçbiri güvenilir değil.
4. **Chapter 9 (Consensus)**: Raft ve Paxos'un neden var olduğunu, ZooKeeper'ın neden production'da bu kadar yaygın olduğunu anlamak.
5. **Chapter 11-12 (Stream Processing & Future)**: Event time / processing time ayrımı ve timeliness ile integrity'nin farklı şeyler olduğu fikri — sistem tasarımına bakışımı en çok değiştiren iki kavram.

**Genel Çıkarımlar**:
- "Database choice" diye bir karar yok; gerçek karar **trade-off seçimi**.
- ACID değil, **hangi specific isolation guarantee** soru sorulmalı.
- Eventually consistent bir sistemde **hangi anomaliyi** kabul ettiğini bilmek gerekiyor.
- Distributed systems'da timeout dışında "doğru cevap" çok az problemde var.
- Schema evolution, production sistemlerde göz ardı edilen ama kritik bir konu.

Kitabın yapısı çok güzel: her bölüm bağımsız da okunabilir ama hep birbirine referans veriyor. Kleppmann'ın hem akademik makalelere hem de pratik production sistemlerine hâkim olması inanılmaz değerli. Hatta her bölüm sonundaki referans listesi başlı başına bir literature review.

## Uygulamalar
DDIA prensiplerini günlük çalışma hayatımda uyguluyorum:
- Database seçimi konuşmalarında "trade-off" diliyle konuşmaya başladım (CP vs. AP, LSM vs. B-tree, leader-based vs. leaderless)
- Code review'larda transaction isolation level'larını sorgular oldum
- Schema migration'larda backward/forward compatibility düşünüyorum
- Replication lag'in user-facing davranışa etkisini consciously düşünüyorum
- "Distributed system" problemlerini debug ederken network, clock ve process pause'u şüpheli olarak görüyorum

## İlgili Kaynaklar
- [Martin Kleppmann's Blog](https://martin.kleppmann.com/) - Yazarın blogu, kitabın takip yazıları
- [DDIA References](https://github.com/ept/ddia-references) - Kitabın referans bibliyografyası
- "Database Internals" by Alex Petrov - DDIA'nın storage bölümünün derinlemesine devamı
- "Streaming Systems" by Tyler Akidau - Chapter 11 için derinlemesine takip
- [The Morning Paper](https://blog.acolyer.org/) - Distributed systems makaleleri özetleri
- [Building Microservices](../building-microservices) - Mikroservis perspektifinden veri yönetimi
- Jepsen Analyses - Production database'lerin consistency garantilerinin testi

## Değerlendirme
- **Öğrenme Değeri**: ⭐⭐⭐⭐⭐ (5/5) — Yazılım mühendisliği kariyerinde olmazsa olmaz bir kaynak
- **Okunabilirlik**: ⭐⭐⭐⭐⭐ (5/5) — Teknik bir kitap için olağanüstü akıcı, illüstrasyonlar mükemmel
- **Tavsiye Edilir mi**: Kesinlikle evet. Backend, distributed systems, veri mühendisliği veya yüksek ölçekli sistemlerle uğraşan herkes için zorunlu okuma. Senior+ seviyeye geçişte gerçek bir milat.
