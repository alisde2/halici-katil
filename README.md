HALICI KATİL — YAMA NOTLARI
Keçeli Studios
=====================================================================

SÜRÜM 2.0.0  —  "Sütunlar"
---------------------------------------------------------------------

YENİ
+ PBR DOKU SETİ HATTI: Yüzeyler artık albedo / normal / roughness üçlüsünü
  arayabiliyor. Set dosyası yoksa prosedürel canvas dokusu aynen kullanılıyor,
  yani 1.8 görünümü bozulmuyor; setler eklendiğinde kod değişmeden devreye
  giriyor (duvar, beton, tuğla, fayans, zemin, ahşap).
+ ORTAM IŞIĞI (IBL): Prosedürel oda ortam haritası — dosya gerektirmiyor;
  koyu kabuk, soğuk tepe paneli ve iki sıcak yan panelden üretiliyor.
  Orta ve üstü kalitelerde açık, Düşük'te tamamen kapalı.
+ SMAA: High kalitede kenar yumuşatma. Ultra zaten 8× MSAA kullandığı için
  orada bilinçli olarak kapalı (1920×1080'de +3,6 ms ölçüldü).
+ FENER KONİSİ: penumbra 0,42 → 0,5 ve gölge normalBias 0,02; büyük gölge
  haritasında duvarlar kendi kendini gölgelemiyor.
+ AÇILIŞ TAKILMASI GİDERİLDİ: GTAO'nun ilk karede 87 ms süren shader/pass
  ısınması yükleme perdesinin arkasına taşındı. Ölçüm (RTX 3060, 1280×720):
  soğuk ilk kare 194–692 ms → 9–30 ms.
+ SU BİRİKİNTİSİ: Çevre haritasını yansıtan parlak su birikintileri
  (Halı Atölyesi'nde üç tane).
+ GÖVDE HAREKET YÖNÜNE DÖNÜYOR: Üçüncü şahısta karakter artık kameranın
  gittiği yöne değil, gerçekten yürüdüğü yöne bakıyor; durunca yumuşakça
  kameraya dönüyor. Strafe ve geri yürüyüş "ay kayması" gibi okunmuyor.
+ AÇIK DURUM TABLOSU: Animasyon klibi seçimi tek yerde (idle / walk / jog /
  sprint / eğilme); davranış 1.8 ile aynı, kod tek noktadan yönetiliyor.
+ İSKELETLİ KARAKTER HATTI: Karakter modeli dosyası varsa prosedürel gövde
  yerine o model geçiyor; animasyon klipleri yeni iskelete taşınıyor
  (retarget) ve silah el kemiğine bağlanıyor. Model yoksa 1.8 yolu birebir.
+ MORPH DUDAK SENKRONU: Model morph hedefi taşıyorsa ağız gerçek morph ile
  açılıyor; yoksa mevcut çene deformasyonu yedek olarak kalıyor.
+ HRTF KONUMSAL SES: Düşman ve uzak adımlar artık gerçekten o yönden geliyor
  (PannerNode, mesafe sönümü panner'ın kendi modelinden). Dinleyici her kare
  kameraya bağlanıyor.
+ ZEMİNE GÖRE ADIM: Çimen, sert zemin, ahşap ve metal için dört ayrı filtre.
  Atölye ve kütüphane ahşap, spor salonu metal, orman çimen.
+ REVERB: Otobüs 0,10/0,9 sn, orman 0,04/0,7 sn, koridor 0,13/1,5 sn.
+ YUMUŞAK YENİDEN DOĞMA: Ölünce bölüm baştan yüklenmiyor. Can dolu başlıyor,
  son sakin noktaya dönüyorsun, avcılar başlangıç noktasına gidip 2,5 sn
  sersemliyor; topladığın eşya ve sigortalar korunuyor.
  Ayarlar → Kontroller'den kapatılabilir, Kâbus'ta zorla kapalı.
+ YEREL ÖLÜM VE SÜRE GÜNLÜĞÜ: Son 80 ölüm (bölüm, aşama, neden, konum,
  zorluk, zaman) ve bölüm süreleri yalnız bu bilgisayarda tutuluyor.
  Hiçbir şey sunucuya gönderilmiyor. `?deaths=1` tabloyu konsola basar.
+ GÜNLÜK GECE TOHUMU: Açıkken Dokuma Gecesi herkes için aynı haritayı kurar.
+ KUMANDA TİTREŞİMİ: Yakalanma ve ani korku anlarında dual-rumble.
+ ?stats=1 GELİŞTİRİCİ HUD'U: Çağrı, üçgen, geometri, doku ve program
  sayısı; kare süresi ve FPS. Yalnız geliştirme derlemesinde.
+ YEREL HATA GÜNLÜĞÜ: Son 25 hata bölüm bilgisiyle bu tarayıcıda saklanıyor;
  `?errors=1` listeler, `?errors=clear` temizler.
+ SIKIŞTIRILMIŞ GLB DESTEĞİ: Bütün model yüklemeleri meshopt çözücüsüyle
  açılıyor; ek paket/dosya gerekmiyor, sıkıştırılmamış dosyalar aynen çalışıyor.

DÜZELTMELER
+ YUSUF'UN İKİNCİ ÖLÜMÜ: Koşu/ölüm sahnesinin kilidi `stage === 'chase'` idi;
  4. bölüm `start` aşamasıyla açıldığı için aynı sahne orada da tetikleniyor
  ve Yusuf ikinci kez ölüyordu. Kilit artık `!trail || stage === 'chase'`.
+ KAMERAYLA GEZEN PARÇACIKLAR KALDIRILDI: Halı Atölyesi'ndeki toz ve 2. bölüm
  ormanındaki sis alanı görüntüyü bozuyordu. Atmosfer FogExp2 ile duruyor;
  sabit konumdaki sis korundu.
+ ?stats=1 "calls 1" gösteriyordu (post-processing her pass'te renderer.info'yu
  sıfırlıyordu). Artık sayaç kare başına ortalama alıyor ve kısıtlı kare
  hızında da güncelleniyor.
+ Bölüm yeniden kurulumunda geometri/doku sayısı artmıyor
  (Kütüphane: 583/54 → 583/54).
+ Eski kayıtlar, başarımlar ve çok oyunculu bozulmadı.

DEĞİŞİKLİKLER
+ Statik gölge önbelleği ölçüldü, uygulanmadı: gölge düşüren tek ışık fener
  ve telefon; ikisi de kameraya bağlı ve her kare hareket ediyor. Önbellek
  gölgeyi kameradan koparırdı, kazanç yoktu.
+ Kalite tablosu genişledi: ortam yoğunluğu ve SMAA kaliteye bağlı.
  Düşük kalite 1.8.0'dan yavaş değil (ch7 Low 1,1 → 1,4 ms, gürültü bandı).
+ Sunucu 2.0.0'a güncellendi: /version ve /health sürümü 2.0.0 döndürüyor.
+ Otomatik güncelleme dosyası (latest.json) 2.0.0 sürüm notlarını ve imzalı
  kurulum adresini taşıyor.

NOT
+ PBR doku setleri, iskeletli karakter modelleri ve strafe/geri yürüme
  klipleri hâlâ dış varlık bekliyor; dosyalar gelene kadar oyun 1.8 yoluyla
  çalışıyor ve görünüm birebir korunuyor.


SÜRÜM 1.8.0  —  "Adı Yoktu"
---------------------------------------------------------------------

YENİ
+ DUDAK SENKRONU: Replik çalarken konuşan karakterin çenesi ve dudakları
  hareket ediyor. Fotoğraf gerçekliğindeki yüzlerde de çene açılıyor;
  iç ses repliklerinde ağız kapanık kalıyor.
+ KONUŞMA DURUŞU: Karakterler replik sırasında boş boş durmuyor, UAL'ın
  konuşma idle animasyonuyla küçük jestler yapıyor.
+ HIZLI AÇILIŞ: Fotoğraf gerçekliğindeki kafalar artık paralel indirilip bir kez
  işleniyor: açılıştan hazır olmaya 6,5 saniye yerine ~1,5 saniye. Karakter
  seçimi ve bölüm başlangıcı kafalar hazır olana kadar bekliyor, yani
  prosedürel yüz bir an görünüp değişmiyor.
+ YUMUŞAK HAREKET KARIŞIMI: Yürüme, koşma ve sprint klipleri hıza göre
  ağırlıkla karışıyor; 3,2 ve 4,2 m/s eşiklerinde klip atlaması yok.
  Kalça yüksekliği yumuşak geçtiği için eğilme/kalkma da sarsılmıyor.
  Ayarlar → Kontroller'den kapatılabilir.
+ FİLMİK RENK DERECELENDİRME: Orta ve üzeri kalitelerde yumuşak kontrast,
  hafif doygunluk, soğuk gölge/sıcak ışık ayrışması ve çok hafif serin
  pus var. Düşük kalitede görünüm birebir eskisi gibi kalır.
+ GERÇEKÇİ HAREKET: Yürüme, koşma ve eğilme animasyonları zemin hızına
  uyuyor; adım kayması azaldı. Kol/bacak salınımı, diz ve dirsek bükülmesi,
  kalça salınımı ve koşarken gövde eğimi eklendi. Animasyon dosyası
  yüklenene kadar kullanılan yedek yürüyüş de yenilendi.
+ 1.8 DEMO PAKETİ: 1-3. bölümler (Otobüs, Kan İzi, Terk Edilmiş Kamp),
  tam sürümün 1.7 içeriğini ve bu sürümün animasyon iyileştirmelerini taşır.
  Çıktı: Çıktılar/Demo/Halıcı Katil Demo 1.8.0.exe ve
  Çıktılar/Demo/Halıcı Katil Demo Kurulum 1.8.0.exe

DEĞİŞİKLİKLER
+ Duru'nun itiraf repliği yeniden yazıldı; artık kendi adını söylüyor.
  TR ve EN ses kayıtları yenilendi.
+ DUDAK HAREKETİ AYARI: Ayarlar → Görüntü'de "Dudak hareketi" anahtarı.
  Kapatınca ağız ve çene hiç oynamaz, kafalar orijinal geometrisinde kalır.
  Çene deformasyonu artık yalnızca ağız hattının altında ve yanak köşeleri
  arasında çalışıyor; genliği küçük bir üst sınırla kısıtlı.
+ DEMO AYRI KİMLİK: Demo paketi artık com.halicikatil.game.demo kimliğiyle
  kuruluyor; tam sürümün üzerine yazmıyor, yanına kuruluyor.
+ 21. bölüm "Dokuma Gecesi"nin 10 repliği seslendirildi; dublaj 207/207.
+ Sunucu 1.8.0'a güncellendi: /version ve /health uçları sürüm, bölüm
  sayısı ve anlık oyuncu sayısını döndürüyor (APP_VERSION ile değiştirilebilir).
+ Otomatik güncelleme dosyası (latest.json) artık 1.8.0 sürüm notlarını
  ve doğru kurulum dosyası adını taşıyor.
+ Oyun tanıtım metni Duru, 19-21. bölümler, Çıkrık rolü ve üç arena
  haritasıyla yeniden yazıldı.

DÜZELTMELER
+ Menüde ve karakter seçiminde kafaların geç yüklenmesi giderildi
  (baş dosyaları tek sefer işlenip önbelleğe alınıyor).
+ Diyalog kapanınca/durdurulunca ağız hareketi de duruyor; sahne
  kapanırken kayıtlı karakterler temizleniyor.
+ Eğilme sırasında yedek animasyonun kalçayı indirmemesi giderildi.
+ Duru'nun itirafındaki eski isim hem metinden hem yeni ses kaydından kaldırıldı;
  oyuncunun görebileceği hiçbir metinde artık geçmiyor.
+ Yumuşak animasyon karışımı kapatıldığında karakterlerin T-pozunda
  donmasına yol açan fadeOut hatası düzeltildi.
+ Bir shader derlenemezse çene deformasyonu kendini kapatıyor ve uyarı
  yazıyor; kafa bozuk çizilmek yerine orijinal hâliyle görünüyor.

---------------------------------------------------------------------
HALICI KATİL — YAMA NOTLARI
Keçeli Studios
=====================================================================

SÜRÜM 1.7.0  —  "Adı Yoktu"
---------------------------------------------------------------------
(1.6.0 hiç yayınlanmadı: 1.6 çalışmasının tamamı bu sürümde çıkıyor.)

YENİ
+ TAM DUBLAJ: 197 replik, Türkçe ve İngilizce, her karakterin kendi sesiyle
  (XTTS-v2). Menüden Kasetler bölümünde 6 kayıt tekrar dinlenebilir.
+ DURU ARTIK KATİL: 1. bölümün otobüs şoförü Duru, o gecenin avcısı.
  Yeni katil görünümü: beyazlamış ten, kanlı kapüşon, tek camı kırık gözlük,
  sargılı kol, sırtında halısı, elinde feneri. Kendi kahkahası ve fısıltısı var.
  Finalde gerçek adını söylüyor; yol boyunca kendini başka bir adla
  tanıtmıştı.
+ SİLAHLAR: balta, çivili sopa, bıçak ve pompalı tüfek elde taşınıyor;
  vuruş düşmanı sersemletiyor, kan lekesi bırakıyor, ekran kanlanıyor.
  Pompalı sesi düşmanları 35 metreye kadar uyandırıyor.
+ BOSS DÖVÜŞÜ: 12. bölümde Duru ile düello. Saldırı öncesi kollarını kaldırıp
  uyarıyor, yarı canda öfkeleniyor, üç vuruşta seni halıya sarıyor.
+ EKRAN KANI: yakalanma, yaralanma ve yakın vuruşlarda kamera kanlanıyor.
+ MÜZİK VE SES EFEKTLERİ: menü, kovalama, orman ve iç mekân ortamları;
  balta, sopa, kapı, sıçrama, kahkaha, çığlık, tüfek ve fısıltı sesleri.
+ YENİ BÖLÜM 19 — "Küçük Halıcı": Duru'nun çocukluğu. Savaş yok; babasının
  atölyesinde 3 hatıra topla, iki dolaba saklan, elinde fener olan adam
  geçerken nefesini tut. Finali bitirince açılır.
+ YENİ BAŞARIMLAR (6, toplam 17): Kırık Gözlük, Kasetleri Dinle, Görünmez,
  Gerçek Son, Baştan Sona, Küçük Halıcı. "Adı Olmayan" kostümü açılır.
+ GİZLİ SON artık gerçek bir tam oyun istiyor: 1. bölümden başla, hiç bölüm
  atlama, bu oyunda gözlüğü ve 6 kasetin hepsini bul, hiç yakalanma.
  Eski kayıttan kalan gözlük/kaset sayılmaz.
+ ÇIKRIK KATİL OLARAK OYNANABİLİR: Kaç-Yakala'da üçüncü katil. Q gücü
  "tezgâh": 6 saniye boyunca her kaçanın yerini duvar arkasından görür.
+ ÜÇÜNCÜ ARENA HARİTASI — "Halı Atölyesi": tezgâhlar, tavandan sarkan
  kilimler, rulo halılar, bölme duvarlar ve atölye lambaları.
+ İZLEYİCİ MODU: kafesteyken Tab ile arkadaşlarını gez (Shift+Tab geri),
  üstte kimin izlendiği yazar; herkes kafesteyken de kamera birine bağlanır.

DEĞİŞİKLİKLER
+ Saklanınca Duru artık dolabı hemen açmıyor: kapının önünde bekliyor.
  Nefesini tut, şişe at, o gidince çık. Zorluk arttıkça daha uzun bekliyor.
+ Erişilebilirlik: altyazı arka planı, renk körlüğü modları (protanopi,
  deuteranopi, tritanopi), sıçramaları ve kamera sarsıntısını azaltma.
+ Dinamik olaylar: elektrik kesintisi (sigortayı indir), sis duvarı,
  uzakta yanıp sönen fener, kapı çarpması. Kâbus'ta daha sık.
+ Halüsinasyonlar: yorgunken koridorda bir şey görünüyor; Ayarlar'dan kapanır.
+ Tahta yazıları artık seçtiğin kahramana göre değişiyor (Ali, Eylül, Azra).
+ Kasetler Duru'nun kayıtları; 12. bölümde Duru ölmüyor, halısını sürükleyip
  karanlığa çekiliyor. Jenerik ve gizli son buna göre yeniden yazıldı.
+ Bölüm 9 kitaplığı tek tek modeller yerine tek bir birleşik modelle çiziliyor
  (donma bitti). Oyun dosyası three.js ayrı pakete alındı, açılış hızlandı.

DÜZELTMELER
* Çıkrık katil seçilince yanında yapay zekâ Duru'nun da gelmesi düzeltildi;
  artık tek avcı var ve oyuncular Çıkrık'ın kendi modelini görüyor.
* Gölgelendirme uyarısı giderildi (three.js'in kaldırdığı yumuşak gölge türü).
* Maç bittiğinde oda olaylarının kaybolması ve izleyici şeridinin ekranda
  kalması düzeltildi; arena sahnesi kapanırken her şey temizleniyor.
* Bölüm 19'da giriş repliği çalınmıyordu; artık anının başında duyuluyor.
* "Görünmez" başarımı 12. bölümden başlayanlara da veriliyordu; artık
  temiz tam oyun gerekiyor. Eski kayıttaki gözlük ve kasetler sayılmıyor.
* Eski kayıtlar korunuyor: replik kimlikleri, kaset kimlikleri ve bölüm
  numaraları aynı; "duru" ile kaydedilmiş oyunlar "azra" ile devam ediyor.

SÜRÜM 1.5.4
---------------------------------------------------------------------
DEĞİŞİKLİKLER
* Ali ile Azra arasındaki yol boyu konuşmalar kaldırıldı; yalnızca final
  sahnesi kaldı. Ali'nin son sözü ve 16. bölümdeki mezar sahnesi buna göre
  yeniden yazıldı ve seslendirildi.
+ Sunucu uyandırma botu: sunucu kendini her 10 dakikada bir uyandırır,
  bağlanırken 1 dakika beklemek gerekmez (sunucu dosyası güncellenmeli).

=====================================================================

SÜRÜM 1.5.3
---------------------------------------------------------------------
YENİ
+ Ayarlar sekmelere ayrıldı: Görüntü, Grafik, Ses, Sesli Sohbet, Kontroller.
+ Yeni ses ayarları: dublaj sesi ve sesli sohbette arkadaşların ses seviyesi.
  Sesli sohbet artık Ayarlar'dan da açılıp kapanabiliyor.
+ Daha gerçekçi 3. şahıs kamera: omuz üstü, baş hizasında, nişan aldığın
  yöne bakar; koşarken uzaklaşır ve görüş açısı genişler, eğilince alçalır,
  yürürken hafif sallanır.

DÜZELTMELER
* Masaüstü kısayolunun eski (Tauri) simgede kalması düzeltildi: kurulum
  bittiğinde Windows'un simge önbelleği yenileniyor.

=====================================================================

SÜRÜM 1.5.2  —  "Gerçek Hareket"
---------------------------------------------------------------------
YENİ
+ Gerçek animasyonlar: karakterler artık gerçek bekleme, yürüme, koşu,
  sprint ve eğilme animasyonlarıyla hareket ediyor (Quaternius Universal
  Animation Library, CC0). Eğilince karakter gerçekten alçalıyor.
  Kıyafetler ve yüzler aynı kaldı.

=====================================================================

SÜRÜM 1.5.1
---------------------------------------------------------------------
DÜZELTMELER
* Ana menüde "Yeni Oyun" ve "Devam et" ayrıldı. Yeni Oyun o kayıt yuvasının
  kaydını, silahlarını ve kurtarılan arkadaşları sıfırlar; Devam et kaldığın
  yerden sürdürür.
* Eski bir oyunda alınan baltanın yeni oyuna taşınması hatası giderildi.
  Silahlar ve kurtarılanlar artık her kayıt yuvasında ayrı tutulur.
  (Başarımlar, kasetler, kostümler ve açılan bölümler korunur.)

=====================================================================

SÜRÜM 1.5.0  —  "İkinci Gece"
---------------------------------------------------------------------
YENİ
+ Hikâye devam ediyor: "2. Gece" (bölüm 16-18, finali bitirince açılır).
  16 Halı Mezarlığı · 17 Çıkrık · 18 Son Düğüm — yeni bir son ve jenerik.
  Ali finalde öldüğü için bu bölümlerde Azra ya da Eylül oynanır.
+ Yeni düşman: Çıkrık, Duru'nun çırağı. Sesi takip eder ve saklandığın
  dolabı bir kez açabilir.
+ Can sistemi: 3 can. Yakalanınca bir can gider ve kaçmak için şansın olur
  (Kâbus zorlukta 1 can).
+ Envanter ve zanaat (I): her bölümde bez, alkol, tel, barut, boru bulunur.
  Bandaj (+1 can), işaret fişeği ve maymuncuk (şifreli kilitleri açar) yap.
+ Birlikte hikâye: biri bölümü bitirince odadaki herkes birlikte geçer.
  Çevrimiçi yakalanırsan "Oyun Bitti" yerine kontrol noktasından dönersin.
+ Tuş atama: Ayarlar > Tuş atama. Tüm tuşlar değiştirilebilir.
+ Yeni dublajlar: eksik 56 replik Türkçe ve İngilizce seslendirildi
  (her karakterin kendi sesiyle, XTTS-v2).

DÜZELTMELER
* Eğilme artık sadece Ctrl.
* ESC oyunu tamamen duraklatır: ses, düşmanlar, ara sahneler durur
  (çok oyunculuda oyun akmaya devam eder).
* 3. şahısta balta, bıçak, sopa ve tüfek artık elin içinde, önünde duruyor.
* Vücut oranları: Ali'nin boyu düzeltildi, Azra'nın kafası küçültüldü,
  omuzlar ve kollar gövdeye birleşti.
* İngilizce dublajlar artık gerçekten çalıyor (dosyalar bulunamıyordu).

=====================================================================

SÜRÜM 1.3.0  —  "Son Fedakârlık"
---------------------------------------------------------------------
YENİ
+ Yeni final sahnesi: Duru son gücüyle Eylül'e saldırır. Azra onun önüne,
  Ali de Azra'nın önüne atlar. Ali, Azra'nın kollarında ölür.
+ Otomatik güncelleme: oyun açılışta yeni sürüm var mı bakar, tek tuşla günceller.
+ Kaç-Yakala:
  - Yakalanan kafese düşer, arkadaşlarını izler (Tab). Arkadaşı E ile kurtarabilir.
  - Katil güçleri (Q): Duru halı fırlatır (yavaşlatır), Davut 5 sn görünmez olur.
  - Yeni harita: Orman Kampı (çadırlar, kamp ateşi). Harita oda sahibince seçilir.
  - Maç sonu skor tablosu.
+ Açık odalar: odayı "açık" kurarsan listede görünür, herkes tıklayıp katılabilir.
+ Eğilme (C / Ctrl): yavaş ve sessiz yürürsün, düşmanlar seni ancak yakından görür.
+ Zemine göre ayak sesi (çimen / sert zemin).
+ Kütüphanede kilitlenebilir kapı, iki not ve şifreli dolap. J: günlük.
+ Yükleme ekranında ipuçları, altyazı boyutu ayarı, FPS sayacı.

DÜZELTMELER
* Seçim ekranında yüzler artık net görünüyor (ışık + yakın kamera).
* Vücutlar yenilendi: daralan kol ve bacaklar, omuz/dirsek/bilek/diz eklemleri,
  tabanlı ayakkabılar; kollar artık gövdenin içinde kaybolmuyor.

=====================================================================

SÜRÜM 1.2.0
---------------------------------------------------------------------
DÜZELTMELER
* Ali ile Azra konuşmaları sadeleştirildi: romantik değil, diğerlerinden
  biraz daha yakın iki arkadaş gibi konuşuyorlar.

=====================================================================

SÜRÜM 1.1.0  —  "Yan Yana"
---------------------------------------------------------------------
YENİ
+ Arkadaş paylaşımı için "Paylaş" klasörü ve kurulum rehberi.

DÜZELTMELER
* Eşya alırken oyunun bir anlığına donması giderildi.
  (Eşyanın parıltı ışığı silinince tüm sahne yeniden derleniyordu.)
* Düşmanlar ilk kez ortaya çıktığında yaşanan takılma giderildi
  (bölüm başında her şey önceden hazırlanıyor).
* Bacaklar ve ayaklar düzeltildi: dizler artık doğru yöne bükülüyor,
  diz eklemi ve bacağa oturan ayakkabılar eklendi.
* 3. şahısta arkadaşlar artık alakasız yönlere koşmuyor; yürüdüğün yöne
  göre hemen yanında, daha yakın yürüyorlar.

=====================================================================

SÜRÜM 1.0.0  —  "Son Düğüm"
---------------------------------------------------------------------
YENİ
+ Hikâye 15 bölüm oldu. Final'i bitirince açılan iki yeni bölüm:
  13 "Davut'un Bodrumu" (kafesler, 3 anahtar) ve
  14 "Yanan Dükkân" (alevler, son halının 3 parçası) — GİZLİ SON burada.
  Yetimhane artık 15. bölüm.
+ Çok oyunculu "Kaç-Yakala" modu: Çok Oyunculu > "Kaç-Yakala başlat".
  Odadaki herkes aynı haritaya girer. Hayatta kalanlar 3 jeneratörü
  çalıştırıp doğudaki kapıdan kaçar; 5 dakika dolarsa katil kazanır.
  Katil oynayan yoksa yapay zekâ Duru avlar.
+ Yeni eşyalar: H = işaret fişeği (10 sn düşmanı uzak tutar),
  T = ayı kapanı (üstüne basan düşmanı 4 sn tutar).
+ 3 kayıt yuvası: Ana Menü > "Kayıt yuvaları" (devam et / yeni oyun).
+ Ana menüde sisin içinde fenerle yürüyen Duru.
+ Final ve gizli sondan sonra kayan jenerik.

DÜZELTMELER
* 3. şahıs kamera artık duvarların içinden geçmiyor (duvara yaklaşır).
* Davut'un Bodrumu biraz aydınlatıldı.
* Bölüm seçme ekranı 15 bölümü gösteriyor.

=====================================================================

SÜRÜM 0.12.0  —  "Oda Kodu"
---------------------------------------------------------------------
YENİ
+ Çok oyunculu artık çok kolay: Ana Menü > Çok Oyunculu > "Oda Kur".
  Oyun 5 haneli bir kod verir; arkadaşın kodu yazıp "Katıl"a basar.
  Sunucu adresi oyuna gömülü (Render), kimsenin bir şey kurması gerekmez.


SÜRÜM 0.11.0  —  "Hepsi Bir Arada"
---------------------------------------------------------------------
YENİ
+ Fotoğraf modu [P]: oyun durur, kamerayla serbestçe uçarsın.
  C: filtre (Siyah-Beyaz, Sepya, Kan, Gece) · Space: fotoğraf kaydet
  (İndirilenler klasörü) · P: çık.
+ Kıyafetler: Seçim ekranında "Kıyafet · Değiştir". Başarımlarla açılır:
  Gece gölgesi, Korucu, Hayatta kalan, Halıcının ceketi.
+ Çok oyunculu 2. aşama: Odada bir oyuncu "oda sahibi" olur ve
  düşmanlar herkeste aynı yerde olur; kapı açma, eşya alma ve
  kurtarmalar herkese yansır.
+ Yakınlık sesli sohbeti: Çok Oyunculu ekranında "Sesli sohbet".
  Yaklaştıkça ses artar, farklı bölümdekiler duyulmaz. M: sustur.
+ Ek Bölüm 13: Yetimhane — Davut'un geçmişi. Oyunu bitirince
  Bölümler ekranında açılır (altyazılı, seslendirmesiz).

NOT: Sunucu güncellendi; Render'a server klasörünün yeni hâlini yükle.


SÜRÜM 0.10.0  —  "Katil Sensin"
---------------------------------------------------------------------
YENİ
+ Oyun tam ekran açılır; F11 ile tam ekrana girip çıkılır
  (Ayarlar'da "Tam ekran" seçeneği de var).
+ Grafik kalitesi: Ultra, Yüksek, Orta, Düşük.
  Ultra: köşe gölgeleri (GTAO), 4K gölgeler, 16x doku filtreleme.
+ Duraklatma menüsünden fare hassasiyeti, parlaklık ve grafik
  kalitesi değiştirilebilir.
+ Görüntü iyileştirmesi: soğuk gölgeler ve sıcak ışıklarla korku
  tonu, yumuşak gölgeler.
+ Çok oyunculu: Duru ya da Davut olarak oyna! Katil rolünde,
  aynı bölümdeki hayatta kalanlara dokunmak onları yakalamaya yeter.
  (Katil rolü bölüm 3, 5, 7, 9, 10 ve 11'de çalışır.)


SÜRÜM 0.9.0  —  "Birlikte"
---------------------------------------------------------------------
YENİ
+ Otobüsten indikten sonra oyun varsayılan olarak 3. şahıs kamerayla
  oynanır ([V] ile 1. şahısa geçilebilir).
+ Arkadaşların yanında yürür: diğer iki başrol ve Rüzgar seni takip eder.
+ Çok oyunculu (ilk aşama): Ana Menü > Çok Oyunculu ekranından bir oda
  koduyla arkadaşlarına bağlan; aynı bölümde birbirinizi görürsünüz.
  Sunucu Render'ın ücretsiz planında çalışır (kurulum: server/README.md).
  Not: Düşmanlar şimdilik her oyuncunun kendi oyununda ayrı çalışır.


SÜRÜM 0.8.0  —  "Oranlar"
---------------------------------------------------------------------
DÜZELTMELER
* Gerçekçi kafalar gövdeye göre küçültüldü; vücut oranları düzeldi.


SÜRÜM 0.7.0  —  "Yeni Yüzler"
---------------------------------------------------------------------
YENİ
+ Yeni karakter: Rüzgar. Azra'nın yanında, finalde Duru'ya karşı
  birlikte duruyor (seslendirmeli).
+ Azra'nın yeni yüzü: uzun açık kahverengi saç.
+ Eylül ve Duru'nun saç ve görünüşleri güncellendi.


SÜRÜM 0.5.0  —  "Azra"
---------------------------------------------------------------------
YENİ
+ Azra artık başrol: Ali ve Eylül ile birlikte oynanabilir ve finalde
  Duru'yla yüzleşmede yer alıyor.
+ Duru başrolden çıktı. Artık kayıp bir sınıf arkadaşı; koridorda
  kırık gözlüğü bulunuyor.

DÜZELTMELER
* Kafa ile gövde arası düzeltildi: 3D kafaların altındaki kendi
  kıyafet ve yaka parçaları boynun en ince yerinden kesildi, kafalar
  doğrudan gövdeye oturuyor.
* Boyunlar inceltildi.
* Eski kayıtlarda Duru ile oynayanlar otomatik olarak Azra ile devam eder.


SÜRÜM 0.4.0  —  "Yüzler"
---------------------------------------------------------------------
YENİ
+ Yan karakterlere gerçekçi 3D yüzler: Meliha, Erberk, Yusuf, Azra,
  Selin, Özge ve Erol.
+ Azra artık siyah saçlı ve mavi gözlü. Azra ölmüyor; geride yalnızca
  tokası kalıyor. Kurtarılabilecek arkadaş sayısı 3 oldu.
+ Yeni karakter seçim ekranı: Ali, Eylül ve Duru sisli bir açıklıkta
  canlı 3D olarak duruyor; seçtiğin karakter fenerin ışığına çıkıyor.
+ Zorluk seviyeleri (Ayarlar): Kolay, Normal, Kâbus.
  Kâbus'ta düşmanlar daha hızlı ve bölüm içi kontrol noktası yok.
+ Fener pili: Fener açıkken pil biter, azalınca titrer.
  Bölümlerde yedek piller var.
+ 11 başarım ve ana menüde "Başarımlar" bölümü.
+ Ana menüde "Fragman" butonu: oyunun kendi görüntülerinden tanıtım.

DÜZELTMELER
* Başrollerde kafa ile gövde arasındaki uyumsuzluk azaltıldı:
  boyun kısaltıldı, ten rengi kafaya uyduruldu, gövde ve kollar
  daha yumuşak hatlı.


SÜRÜM 0.3.0  —  "Hepimiz"
---------------------------------------------------------------------
YENİ
+ Arkadaşlarını kurtar: Selin, Özge ve Erberk yardım istediğinde
  7 saniye içinde yanlarına gidip [E]'ye basarsan hayatta kalırlar.
+ 3 farklı son: Kaç kişiyi kurtardığına göre final değişir
  ("Yalnız", "Yaralı", "Hepimiz"). Son ekranında hayatta kalanlar
  ve kaybedilenler ayrı ayrı yazılır.
+ Şişe fırlatma [G]: Şişe düştüğü yerde kırılır, Duru ve Davut
  sesin geldiği yere gider. Bölümlerde yeni şişeler bulunabilir.
+ Nefes tutma: Dolapta saklanırken düşman yaklaşınca [Space] basılı
  tut. Nefesin biterse ya da düşmanın dibinde soluk alırsan duyulursun.
+ 6 kaset kaydı: Duru'nun geçmişini anlatan kasetler bölümlere
  saklandı. Ana menü > Kasetler bölümünden tekrar dinlenebilir.
+ Gamepad desteği (Xbox / PlayStation):
  Sol çubuk hareket, sağ çubuk bakış, A etkileşim, B nefes tut,
  X / RT saldırı, Y fener, LB şişe, LT koşma, Start duraklat.
+ Yeni kapak görseli ve uygulama simgesi; ana menü arka planı yenilendi.

DUBLAJ
* Tüm Türkçe replikler daha net bir ses motoruyla (ElevenLabs)
  yeniden seslendirildi. Anlaşılmayan kelimeler düzeltildi.
* İngilizce dublaj tamamlandı (140 / 140 replik).
* Her karakterin kendine özel sesi var:   Ali, Eylül, Duru, Meliha,
  Davut, Ebru Hoca, Erberk, Erol, Yusuf, Azra, Selin, Özge.

TEKNİK
* Her yeni sürüm dosya adına yazılıyor (ör. "Halıcı Katil 0.3.0.exe").
* Oyun boyutu sınırı 1 GB'a çıkarıldı.


SÜRÜM 0.2.0  —  "On İki Bölüm"
---------------------------------------------------------------------
YENİ
+ Hikâye 12 bölüme genişletildi: Otobüs, Kan İzi, Terk Edilmiş Kamp,
  Korucu Kulübesi, Okul Bahçesi, Koridor, Halı Atölyesi, Jeneratör,
  Kütüphane, Çatı, Spor Salonu ve final: Büyük Salon.
+ Bölüm seçme ekranı, kontrol noktaları ve "Devam Et".
+ Gerçekçi karakter yüzleri ve yenilenen otobüs.
+ Elde tutulan silahlar ve vuruş mekaniği: balta, çivili sopa,
  bıçak, pompalı tüfek. Vurulan düşman kısa süre sersemler.
+ Her arkadaşın kendine özel ölüm sahnesi (halıya sarılma,
  sürüklenme, çatıdan düşme, çadırda saldırı).
+ Keçeli Studios açılış ekranı ve daha korkunç ana menü.
+ Korkutucu müzikler, ortam sesleri ve çığlıklar.
+ Türkçe ve İngilizce dublaj (ilk sürüm).

DÜZELTMELER
* Katil kovalarken ekrandaki kan efekti küçük lekelere dönüştü.
* Ormandaki görüntü karıncalanması ve pikselleşme giderildi.
* Oyun genel olarak daha parlak; gece teması korunarak görünürlük arttı.
* Parlaklık ayarı eklendi.

=====================================================================
Geri bildirim için: Keçeli Studios
