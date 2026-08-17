# Clean Architecture

## Genel Bilgiler
- **Yazar**: Robert C. Martin (Uncle Bob)
- **Yayın Yılı**: 2017 (Prentice Hall)
- **Format**: PDF/EPUB
- **Durum**: ✅ Tamamlandı

## Özet
Uncle Bob'un yazılım mimarisini "detayları ertelemek" üzerine kurduğu kitap. SOLID prensiplerinden başlayıp component prensiplerine, oradan da meşhur konsantrik daire diyagramına (Entities → Use Cases → Interface Adapters → Frameworks & Drivers) uzanıyor. Ana tez net: iş kuralları merkeze konur, veritabanı, web ve framework birer **detaydır** ve mimari bu detaylara bağımlı olmamalıdır. Bağımlılıklar her zaman içeriye, yüksek seviye politikaya doğru bakar (**Dependency Rule**).

## Ana Konular
- [x] **Part I-II: Giriş ve Programlama Paradigmaları**
  - İki değer: davranış (behavior) vs. yapı (structure)
  - Structured, OO ve Functional programlamanın her birinin bize bir şeyi *yasakladığı* fikri
- [x] **Part III: SOLID Prensipleri**
  - SRP, OCP, LSP, ISP, DIP
- [x] **Part IV: Component Prensipleri**
  - Cohesion: REP, CCP, CRP
  - Coupling: ADP, SDP, SAP + tension diagram
- [x] **Part V: Mimari**
  - Bağımsızlık, sınırlar (boundaries), policy & level, business rules
  - Screaming Architecture, The Clean Architecture, Humble Object
  - Main component, servisler, test boundary
- [x] **Part VI: Detaylar**
  - Veritabanı, web ve framework birer detay
  - Case study + Simon Brown'un "The Missing Chapter"ı

## Okuma Planı
### Başlangıç Tarihi
Haziran 2026

### Bitiş Tarihi (Hedef)
Ağustos 2026

### İlerleme
- [x] Başlangıç
- [x] %25 tamamlandı
- [x] %50 tamamlandı
- [x] %75 tamamlandı
- [x] ✅ Tamamlandı

## Bölümler ve Notlar

### Part I-II: Paradigmalar
- **İki değer**: Yazılımın *davranışı* acil ama önemsiz, *yapısı* acil değil ama önemli olabiliyor. Sadece acil olana koşan ekipler mimariyi kaybediyor.
- Üç paradigmanın ortak yanı: hepsi programcıya bir yetki **ekler değil, çıkarır**. Structured → goto'yu, OO → function pointer'ın kontrolsüz kullanımını, Functional → atamayı (assignment) yasaklıyor.
- OO'nun asıl gücü encapsulation veya inheritance değil, **polymorphism ile bağımlılığın yönünü tersine çevirebilmek** (plugin mimarisi).

### Part III: SOLID
- **SRP**: "Bir modülün tek bir aktöre karşı sorumlu olması." Klasik "tek iş yapsın" yorumu eksik — mesele *değişim sebebi*.
- **OCP**: Genişletmeye açık, değişime kapalı. Bileşenleri seviyelere ayırıp yüksek seviyeyi düşük seviyedeki değişimden koruma.
- **LSP**: Alt tipler yerine geçebilmeli; ihlali mimari seviyede `if (type == X)` hack'leri olarak geri dönüyor.
- **ISP**: Kullanmadığın şeye bağımlı olma — dil seviyesinden çok bağımlılık/deploy seviyesinde önemli.
- **DIP**: Kitabın kalbi. Somut olana değil, soyutlamaya bağımlı ol. Abstract factory ile "kaynak kodu bağımlılığını akış yönünün tersine çevirme".

### Part IV: Component Prensipleri
- **Cohesion**: REP (release ile aynı granülarite), CCP (birlikte değişen sınıflar birlikte paketlensin — component seviyesinde SRP), CRP (birlikte kullanılmayanlar ayrılsın). Üçü aynı anda maksimize edilemiyor, gerilim var.
- **Coupling**: ADP (bağımlılık grafiğinde döngü olmasın — DIP ile kırılır), SDP (stabil olana doğru bağımlı ol), SAP (stabil olan soyut olmalı).
- **Main sequence / D metriği**: "Zone of pain" (stabil + somut, örn. veritabanı şeması) ve "zone of uselessness" (soyut ama kimse kullanmıyor) ayrımı çok işe yarar bir ölçüm.

### Part V: Mimari
- **Mimarinin amacı**: Sistemi build/deploy/operate/maintain etmek için gereken **insan kaynağını minimize etmek** — performansı değil.
- **İyi mimar, alınmamış karar sayısını maksimize eder**: DB, framework, web sunucusu seçimini olabildiğince geciktir. Bu "deferring decisions" fikri kitabın en pratik çıkarımı.
- **Boundaries**: Sınırlar, gerçekten değişme hızları farklı olan yerlere çizilir. Sınır çizmenin maliyeti var — her yere çizmek de over-engineering.
- **The Clean Architecture**: Entities (enterprise business rules) → Use Cases (application business rules) → Interface Adapters (controller, presenter, gateway) → Frameworks & Drivers. **Dependency Rule**: kaynak kodu bağımlılıkları yalnızca içeriye doğru.
- **Sınır geçişleri**: İçeriden dışarıya çağrı gerektiğinde DIP + **Humble Object** (test edilebilir mantık ile test edilemez GUI/IO'yu ayırma). Presenter/View ayrımı bunun tipik örneği.
- **Screaming Architecture**: Klasör yapısı "bu bir Rails/Spring uygulaması" değil, "bu bir muhasebe sistemi" diye bağırmalı.
- **Test boundary**: Testler de sistemin bir bileşeni; GUI'ye bağlı kırılgan testler (Fragile Test Problem) mimari bir hata.

### Part VI: Detaylar
- **Veritabanı bir detaydır**: Veri modeli mimariye ait, veritabanı ürünü değil. İş kuralları SQL'i bilmemeli.
- **Web bir detaydır**: UI sadece bir I/O device'ı; sunum teknolojisi 5 yılda bir değişiyor, iş kuralları değişmiyor.
- **Framework'ler detaydır**: Framework yazarı senin değil, kendi problemini çözüyor. Framework'e "evlenmeden" mesafeli kal, kalıtım yerine kompozisyonla kullan.
- **The Missing Chapter (Simon Brown)**: Kitabın en somut bölümü. Package by layer / by feature / ports & adapters / **by component** karşılaştırması ve "the devil is in the implementation details" — access modifier'ları düzgün kullanmazsan çizdiğin bütün sınırlar kağıt üzerinde kalıyor.

## Notlar

### Önemli Alıntılar
- "The goal of software architecture is to minimize the human resources required to build and maintain the required system."
- "A good architect maximizes the number of decisions not made."
- "The database is a detail."
- "Your architecture should tell readers about the system, not about the frameworks you used in your system."
- "Making messes is always slower than staying clean."

### Kişisel Çıkarımlar
Clean Code'un mimari seviyedeki devamı gibi okunuyor; ikisi arasındaki köprüyü SOLID kuruyor. En çok işime yarayan fikir "detayları erteleme" oldu — bir projeye veritabanı seçimiyle başlamanın aslında ne kadar erken ve gereksiz bir taahhüt olduğunu net gösteriyor.

Component prensipleri (Part IV) beklemediğim kadar değerliydi; SOLID kadar konuşulmuyor ama modül/paket sınırlarını tartışırken elimde somut bir dil oluyor artık (stable dependencies, döngüsel bağımlılık, zone of pain).

Eleştirim: Kitap tekrar yoğun ve bazı bölümler (özellikle paradigma tarihçesi ve Appendix'teki mimari arkeolojisi) fikirden çok anı anlatıyor. Ayrıca çizilen ideal mimarinin maliyeti yeterince dürüst tartışılmıyor — her sınır ekstra indirection demek. Simon Brown'un ek bölümü tam da bu boşluğu doldurduğu için kitabın en iyi kısımlarından biri.

## Uygulamalar
- Yeni modüllerde iş kurallarını framework/DB tiplerinden bağımsız tutmaya, bağımlılıkları interface üzerinden içeriye çevirmeye çalışıyorum.
- Klasör yapısını teknik katman (controllers/, models/) yerine feature/component bazlı düşünmeye başladım.
- Code review'larda "bu bağımlılık hangi yöne bakıyor?" ve "bu karar şimdi verilmek zorunda mı?" sorularını soruyorum.
- Testleri UI'a değil use case seviyesine bağlayarak kırılganlığı azaltıyorum.

## İlgili Kaynaklar
- [Clean Code Collection](../clean-code-collection) — Aynı yazar, kod seviyesindeki karşılığı
- [Agile Software Development](../agile-software-development) — SOLID ve component prensiplerinin ilk ve daha detaylı anlatımı
- [Dependency Injection](../dependency-injection) — DIP'in pratik uygulaması
- [Domain-Driven Design](../domain-driven-design) — Merkezdeki "entities" katmanının domain modeli karşılığı
- [Patterns of Enterprise Application Architecture](../patterns-of-enterprise-application-architecture) — Interface adapter katmanındaki desenler
- [The Clean Code Blog](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) — Kitabın çekirdek makalesi
- Alistair Cockburn — Hexagonal Architecture (Ports & Adapters)

## Değerlendirme
- **Öğrenme Değeri**: ⭐⭐⭐⭐ (4/5) — Mimari düşünmeye net bir çerçeve veriyor, ancak fikir yoğunluğu sayfa sayısına göre düşük
- **Okunabilirlik**: ⭐⭐⭐⭐⭐ (5/5) — Kısa bölümler, bol örnek, çok akıcı
- **Tavsiye Edilir mi**: Evet. Özellikle Part III-V, mid-level'dan senior'a geçişte bağımlılık yönetimi konusunda ortak bir dil kazandırıyor. Part VI ve Simon Brown'un ek bölümü mutlaka okunmalı.
