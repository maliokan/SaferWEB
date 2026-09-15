SaferWEB — Ortak güvenlik motoru, CLI ve mobil geliştirme planı
=============================================================

Belge revizyonu: 2 — CLI ve Android emülatörü kapsamı
Planın başlangıç tarihi: 9 Eylül 2026
Güncelleme tarihi: 10 Eylül 2026

Bu revizyon, kullanıcının esas alınmasını istediği planı son kararlarına göre günceller. Uygulama kodu ve depo henüz oluşturulmamıştır; bu çalışmada kullanıcının bilgisayarına araç kurulmamıştır. Aşağıdaki sweb komutları geliştirilecek arayüzün sözleşmesidir; henüz çalışan bir ürün komutu değildir. Süreler tahmin, başarı oranları henüz ölçülmemiştir.

**Bu revizyonda kararlaştırılan değişiklikler**

- Projenin adı her yerde SaferWEB.
- Aynı Python motorunu kullanan iki ürün yüzeyi: masaüstünde sweb CLI ve Android mobil uygulama.
- İlk kullanılabilir teslimat GitHub'dan kurulabilen CLI; mobil uygulama sonraki aşamada aynı motora bağlanacak.
- Kullanıcının istediği sweb -Xd www.example.com biçimi desteklenecek.
- CLI, kendi bilgisayarında analiz yapacak; SaferWEB sunucusu veya harici güvenlik API'si gerektirmeyecek.
- Android geliştirme/test ortamı Android Studio içindeki Android Emulator/AVD olacak.
- Bilinen ortam: CachyOS, VS Code, varsayılan Fish, Node v26.8.1, npm 12.0.2.
- Masaüstü kapsamı bu revizyonda terminal uygulaması olarak ele alınıyor. Pencereli bir masaüstü arayüzü ayrıca planlanmış değil.

**Doğrulanmış kullanıcı ortamı ve proje hedefleri**

| Bileşen | Kullanıcının bildirdiği durum | Plan |
| --- | --- | --- |
| İşletim sistemi | CachyOS / Linux | İlk desteklenen CLI geliştirme ortamı. |
| Editör | VS Code kurulu | Python, CLI, API ve mobil kod aynı editörde geliştirilecek. |
| Shell | Varsayılan Fish | CLI Fish ve Bash'te test edilecek; shell değiştirmek gerekmiyor. |
| Node | v26.8.1 | Mobil proje için ayrıca Node 24 LTS seçilecek; mevcut kurulumun kaldırılması gerekmiyor. |
| npm | 12.0.2 | Mevcut sürüm kaydı; Node 24 ortamında beraber gelen uyumlu npm kullanılacak. |
| Mobil test | Android Studio emülatörü seçildi | Fiziksel telefon ilk geliştirme ortamının ön koşulu değil. |
| Diğer araçlar | Kurulumları henüz doğrulanmadı | Git, uv, Python, SDK/JDK ve Docker uygun aşamada kontrol edilecek. |

**Projenin hedefi**

SaferWEB, kullanıcının girdiği URL veya QR içindeki bağlantıyı analiz ederek kimlik avı, aldatıcı alan adı, şüpheli yönlendirme ve zararlı dosya indirmeye yönlendirme işaretlerini açıklayan açık kaynak bir güvenlik projesidir.

İlk ürün terminalden kullanılan sweb komutu olacak. Kullanıcı GitHub deposunu indirip gerekli kurulum adımını tamamladıktan sonra kendi bilgisayarında bağlantı kontrolü yapabilecek. Bash'te çalışması, analiz motorunun Bash ile yazılmasını gerektirmez: kurulan Python komutu Fish, Bash ve desteklenen diğer shell'lerden çağrılır.

Mobil ürün Android ile başlayacak; kullanıcı URL girecek veya QR okutacak. Mobil uygulama FastAPI üzerinden aynı Python motorunu kullanacak. iOS'a genişleyebilecek arayüz mimarisi korunacak. QR, güvenlik motorunun giriş yöntemlerinden biri olmaya devam edecek.

CLI'nin yerelde çalışması ve çevrimdışı çalışması farklı özelliklerdir. Yerel CLI, ağlı analizde kullanıcının bilgisayarından hedefe DNS/TLS/HTTP erişimi yapar. Çevrimdışı profil yalnızca URL metnini ve paketlenmiş yerel kuralları inceler. İlk mobil sürümün tam analizi backend bağlantısı gerektirir.

Uygulama bir bağlantının bütünüyle güvenli olduğunu ispatlayamaz. Yeni saldırılar, ele geçirilmiş meşru siteler, zamana veya ziyaretçiye göre değişen içerik ve JavaScript ile yüklenen davranışlar görünmeyebilir. Sonuç kullanılan analiz profilinin kapsamını açıkça belirtir. Bir indirme bağlantısı, dosyanın zararlı olduğuna dair tek başına kanıt değildir.

GitHub'daki teknik değer; bağımsız kurulabilen CLI, açıklanabilir ortak motor, güvenli ağ erişimi, ölçülmüş yanlış alarm oranı ve tekrar üretilebilir değerlendirmeden oluşacak.

**İlk sürümün kabul edilen kapsamı**

| Konu | Davranış |
| --- | --- |
| Masaüstü | Linux terminalinde sweb komutu; Fish ve Bash üzerinde destek/test. |
| CLI girdi | Tek alan adı veya tam HTTP/HTTPS URL; hassas URL için tek girdilik stdin seçeneği. |
| Yerel motor | CLI, Python motorunu kendi sürecinde kullanır; API sunucusu başlatmak gerekmez. |
| Ağlı analiz | -X / --extended ile sınırlı DNS/TLS/HTTP, yönlendirme ve hazır olduğunda statik HTML. |
| Çevrimdışı analiz | -X verilmediğinde ağsız profil; --offline bunu açıkça zorlar. |
| Çıktı | İnsan için terminal raporu; otomasyon için JSON ve belgelenmiş çıkış kodları. |
| Mobil URL | Android uygulamasında elle giriş veya bilinçli yapıştırma. |
| QR | Android'de kamera içeriğini çözme, tür/alan adı önizlemesi; görüntü backend'e gönderilmez. |
| URL dışındaki QR | Wi-Fi, telefon, SMS, kişi kartı ve diğer türleri gösterme; otomatik işlem başlatmama. |
| Mobil ağ analizi | Kullanıcının analiz eylemiyle kendi FastAPI servisine istek. |
| Eksik analiz | Zaman aşımı, erişim engeli ve çalıştırılmayan kontrollerin raporlanması. |
| Link açma | CLI kendiliğinden tarayıcı açmaz; mobilde açık kullanıcı eylemi gerekir. |
| Geçmiş | CLI varsayılan olarak geçmiş tutmaz; mobilde isteğe bağlı cihaz içi geçmiş sonraki küçük sürüme eklenebilir. |
| Hesap ve itibar API'si | Yerel CLI için hesap/API anahtarı gerekmez. Harici itibar verileri isteğe bağlıdır. |

İlk kapsam; tarayıcı uzantısı, ağdaki tüm cihazların taranması, exploit/vulnerability taraması, JavaScript çalıştırma, dosya çalıştıran malware sandbox'ı ve pencereli masaüstü uygulaması içermez. Windows/macOS CLI desteği ayrıca test edilmeden destekleniyor diye ilan edilmeyecek.

**Önerilen teknoloji kararları**

| Katman | Seçim | Gerekçe |
| --- | --- | --- |
| Ortak analiz motoru | Python 3.13 | CLI ve API'nin birlikte kullandığı güvenlik mantığı. |
| CLI seçenekleri | Click | Kısa/uzun seçenekler, yardım, hata yönetimi ve -Xd gibi birleşik seçenekler. |
| Terminal sunumu | Rich | Okunabilir rapor ve kontrollü renkli çıktı; JSON standart JSON serileştiriciyle üretilir. |
| Ortak veri modelleri | Pydantic | Analiz isteği, bulgu ve sonuç sözleşmesi; FastAPI'den bağımsız kullanılır. |
| Python ortamı | uv | Yönetilen Python, .venv, kilit dosyası ve kurulabilir CLI. |
| Paketleme | pyproject.toml, src düzeni, console entry point | Python paketinin sweb komutunu sağlaması. |
| Mobil | React Native + Expo + TypeScript | Android arayüzü ve iOS'a genişleme olanağı. |
| Mobil gezinme / QR | Expo Router / expo-camera | Ekran akışı ve kamera/QR. |
| Mobil API | FastAPI + Uvicorn | Mobil istemciye HTTP arayüzü; Python paketinde isteğe bağlı api bağımlılık grubu. |
| Node / npm | Mobil proje için Node 24 LTS ve beraber gelen npm | Kullanıcıdaki Node 26 ile proje sürümünü fnm üzerinden ayırma. |
| Mobil test | Android Studio + Android Emulator/AVD | Kullanıcının seçtiği Android test cihazı. |
| Yerel Android derleme | OpenJDK 17 ve uyumlu SDK/Build-Tools | Expo projesinin yerel development build'ini derlemek için. |
| Ağ toplama | Ortak kısıtlı Python ağ modülü | CLI yerelde kullanır; API dağıtımında izole işçi kullanır. |
| Sunucu / laboratuvar | Docker Engine + Compose | API işçisinin izolasyonu ve tekrarlanabilir test ağı; normal CLI kurulumu için zorunlu değil. |
| Test / kalite | pytest, Hypothesis, Ruff; TypeScript ve ESLint | Motor, CLI sözleşmesi, güvenlik sınırları ve mobil akışlar. |
| Sürüm kontrolü | Git + GitHub | Kod, sorunlar, yayınlar ve CI. |
| Veritabanı | İlk CLI'de zorunlu veritabanı yok | Kalıcı sunucu işleri doğarsa PostgreSQL ayrıca değerlendirilir. |

Click, sonuncusu değer alan kısa seçeneklerin birleştirilmesini destekliyor; bu nedenle -X bayrağı ve -d alan adı seçeneği -Xd şeklinde tasarlanabilir. Rich terminalin özelliklerine uygun sunum sağlar. [Click seçenekleri](https://click.palletsprojects.com/en/stable/options/), [Rich Console](https://rich.readthedocs.io/en/stable/console.html).

Python paketleme standardındaki console entry point, kurulumdan sonra sweb gibi bir komut sağlamaya uygundur. Motor, CLI ve isteğe bağlı API tek Python dağıtımı içinde modüllere ayrılır; mobil uygulama aynı GitHub deposunda ayrı bir npm projesidir. [Python CLI paketleme rehberi](https://packaging.python.org/en/latest/guides/creating-command-line-tools/).

Expo Linux ve Node LTS kullanımını destekliyor; QR desteği expo-camera içinde bulunuyor. [Expo proje kurulumu](https://docs.expo.dev/get-started/create-a-project/), [Expo Camera](https://docs.expo.dev/versions/latest/sdk/camera/).

Kullanıcının mevcut Node 26.8.1 sürümü kayda alındı. Planın kontrol edildiği tarihte Node 26 Current, Node 24 LTS hattında; mobil proje Node 24 ile sabitlenecek. Bu karar CLI'yi etkilemez: CLI çalıştırmak için Node/npm kurulumu gerekmez. [Node sürüm tablosu](https://nodejs.org/en/about/previous-releases).

Linux'ta Android derlenebilir. Yerel iOS derlemesi ve simülatörü macOS/Xcode gerektirir; iOS sonraki aşamadır. [React Native ortamı](https://reactnative.dev/docs/set-up-your-environment).

**Mimarinin sorumlulukları**

| Bileşen | Sorumluluk | Nerede çalışır? |
| --- | --- | --- |
| sweb CLI | Parametreler, stdin, yardım, rapor ve çıkış kodları | Kullanıcının bilgisayarı |
| Core | URL doğrulama, özellikler, kurallar, kanıt ve skor | CLI veya API işçisi |
| Network | DNS/IP güvenliği, TLS, kısıtlı HTTP, yönlendirme ve gövde sınırları | CLI'de yerel; sunucuda ayrı işçi |
| FastAPI | Mobil istek doğrulama, limitler ve sonuç sözleşmesi | Yerel geliştirme makinesi veya sunucu |
| Mobil | URL/QR alma, önizleme, analiz isteği ve sonuçlar | İlk aşamada Android emülatörü |
| Veri adaptörleri | İsteğe bağlı itibar verisi ve sürümlü güncellemeler | Açıkça etkinleştirilen güncelleme katmanı |
| Evaluation | Veri/test ayrımı, katkı ve başarı ölçümleri | Geliştirici ortamı ve CI |

CLI, core'u doğrudan Python fonksiyonlarıyla çağırır. Mobil uygulama kendi FastAPI arayüzümüze gider; API aynı motor sürümünü çalıştıran işçiye işi verir. API, sweb alt süreci başlatıp terminal çıktısını ayrıştırmayacak. Bu iki arayüz için ayrı güvenlik algoritmaları yazılmayacak.

Çekirdek kurallar ağ istemcisinden ve kullanıcı arayüzünden ayrılacak. Ağ verisi toplayıcıdan gelir; motor testlerde önceden hazırlanmış aynı gözlemlerle çalışabilir. Aynı girdiler, gözlemler, kurallar ve sürümle CLI/API aynı bulguyu üretmeli. Farklı makinelerdeki canlı taramalarda IP/konum/zaman nedeniyle içerik farklı çıkabilir; bu eşitlik iddiasının sınırı raporda korunur.

CLI temel kurulumu core, CLI ve kısıtlı ağ modülünü içerir. FastAPI/Uvicorn isteğe bağlı api kurulumu ile eklenir. Docker, Android SDK ve npm bağımlılıkları CLI kullanıcılarının kurulumuna taşınmaz.

CLI başlangıçta normal kullanıcı yetkisiyle çalışan bir uygulamadır; ayrı bir sandbox içinde olduğu varsayılmaz. Sunucu tarafında ağ işçisi sırlar ve kontrol servislerinden ayrılmış konteyner/ağ politikasına sahip olur. Her iki mod aynı hedef doğrulama ve kaynak sınırlarını uygular.

**sweb komut sözleşmesi**

Aşağıdaki kullanım tasarımı, uygulama geliştirilirken kabul testlerine dönüşecek:

    sweb -Xd www.example.com

Bu biçim aşağıdaki iki biçimle aynı anlama gelecek:

    sweb -X -d www.example.com
    sweb --extended --domain www.example.com

| Seçenek | Anlamı |
| --- | --- |
| -d, --domain ALAN_ADI | Yalnızca alan adı/host alır; şema, yol, sorgu veya kullanıcı bilgisi kabul etmez. |
| -u, --url URL | Tam HTTP/HTTPS URL alır. Yol/sorgu anlamı korunur. |
| -X, --extended | Ağ erişimini etkinleştirir: DNS, TLS, sınırlı yönlendirme ve statik HTML analizi. |
| --offline | Ağ erişimini tamamen kapatır; -X ile birlikte verilirse kullanım hatasıdır. |
| --stdin | Tek URL'yi standart girdiden alır; -d/-u ile birlikte kullanılmaz. |
| --json | Sonucu tek JSON belgesi olarak stdout'a yazar. |
| --no-color | İnsan raporunda terminal renklerini kapatır. |
| --help | Seçenekleri, kapsamı ve örnekleri gösterir. |
| --version | Araç/motor sürümünü gösterir. |

-d, -u ve --stdin girdilerinden tam olarak biri seçilir. --help ve --version için hedef gerekmez. İlk sürüm tek hedefi inceler; toplu dosya, alt ağ veya kapsamlı site crawler'ı eklenmez.

-X verilmediğinde varsayılan URL metni analizi ağsızdır. -X hiçbir güvenlik sınırını kaldırmaz; exploit taraması ya da JavaScript çalıştırma anlamına gelmez. -Xd, iki ayrı kısa seçeneğin birleşimidir; d değer aldığı için bu birleşimde sonda kalır.

Alan adı girdisi ağlı incelemede https:// ile ve kök yol üzerinden ele alınır; www otomatik eklenmez veya kaldırılmaz. HTTPS başarısız olunca gizlice HTTP'ye düşülmez. Kullanıcı açık bir HTTP URL'si inceleyecekse -u kullanır. Sonuçta gerçek istek URL'si ve profil görünür olur.

Örnek kullanım tasarımları:

    sweb --offline -d www.example.com
    sweb -X -u 'https://www.example.com/login?source=qr&lang=tr'
    sweb -Xd www.example.com --json
    sweb --offline --stdin
    sweb --help

URL içindeki &, ?, * gibi karakterler shell tarafından yorumlanabildiği için tam URL örnekleri tek tırnak içinde gösterilecek. Terminale argümanla verilen hassas URL shell geçmişine yazılabilir; --stdin seçeneği böyle bir bağlantıyı doğrudan komut satırına yazma ihtiyacını azaltır.

--offline çalışma sırasında hedef DNS sorgusu, HTTP isteği, telemetri, PSL/marka listesi güncellemesi veya itibar servisi çağrısı yapmaz. Kurulumun paket indirmesi, kurulu aracın offline çalışma sözleşmesinden ayrıdır. Yerel listeler sürümü belli kopyalardan okunur.

**CLI çıktı ve otomasyon sözleşmesi**

İnsan raporu; hedef alan adı, seçilen profil, risk işaretleri, gerekçeler ve tamamlanmayan kontrolleri gösterir. JSON raporu aynı bilgiye ek olarak schema_version, engine_version, ruleset_version, profile, findings, risk_state, analysis_state ve kontrol kapsamını taşır.

stdout sonuç için, stderr ilerleme/uyarı/hata mesajları için kullanılacak. --json seçiliyken stdout'a banner, spinner veya ANSI renk dizisi karışmayacak. Parametre hatalarında JSON sonuç oluşmadan stderr ve çıkış kodu 2 dönmesi belgelenir.

| Çıkış kodu | Sözleşme |
| --- | --- |
| 0 | İstenen profil tamamlandı ve şüpheli/yüksek risk bulgusu yok; güvenlik garantisi değildir. |
| 1 | Şüpheli veya yüksek risk eşiğini karşılayan bulgu var; analiz kısmi olsa da risk görünür kalır. |
| 2 | Yanlış parametre, çelişen seçenek veya geçersiz hedef biçimi. |
| 3 | Politika engeli, gerekli kontrolün zaman aşımı veya yetersiz kanıt nedeniyle istenen değerlendirme tamamlanamadı; eşik üstü bulgu yok. |
| 4 | Beklenmedik iç uygulama hatası. |
| 130 | Kullanıcı işlemi Ctrl+C ile iptal etti. |

Parametre hatası/iç hata/iptal kendi kodunu alır. Geçerli rapor üretildiyse risk eşiği varsa 1, yoksa değerlendirme yetersiz/kısmi ise 3, diğer durumda 0 seçilir. Çevrimdışı profilin tamamlanması ağlı profilin tamamlandığı anlamına gelmez; ikisi raporda ayrı tanımlanır. Belirsiz bir risk durumu yalnızca hata oluşmadı diye 0'a çevrilmez.

Raporlara dış kaynaklı ham HTML veya terminal kontrol karakterleri basılmaz. URL, başlık ve kanıt metnindeki ANSI/OSC dizileri, kontrol karakterleri ve yanıltıcı yön işaretleri güvenli gösterime çevrilir. Rich markup olarak yorumlanmaz; hassas parametreler görüntüleme/log katmanında maskelenir.

**GitHub'dan kurulum ve dağıtım hedefi**

İlk dağıtım yolu GitHub kaynak deposu veya GitHub Release kaynak arşivi olacak. Kullanıcıdaki gerçek repo adresi henüz verilmediği için aşağıdaki GITHUB_KULLANICISI bir yer tutucudur. Bu komutlar kaynak kod/paketleme tamamlandıktan sonraki kurulum deneyimini tarif eder:

    git clone 'https://github.com/GITHUB_KULLANICISI/SaferWEB.git'
    cd SaferWEB
    uv tool install --python 3.13 .

Kurulumdan sonra Fish'te araç dizinini PATH'e eklemek gerekirse:

    fish_add_path (uv tool dir --bin)

Bash için uv tool update-shell ve yeni terminal açma yolu belgelenir. fish_add_path Bash komutu değildir. Asıl sweb komutu her iki shell'de aynı seçenekleri kullanır.

Kurulum sonrasında herhangi bir çalışma dizininden:

    sweb --help
    sweb -Xd www.example.com

Hedef, kullanıcının her çağrıda python dosya.py yazmamasıdır. pyproject.toml içindeki console entry point kurulunca sweb komutu üretilir. uv aracın Python ortamını izole eder. Bu ilk yöntem Python/uv tabanlı kurulumdur; tek başına taşınabilen bağımlılıksız bir binary vaadi değildir. [uv araç kurulumu](https://docs.astral.sh/uv/guides/tools/), [uv tool komutları](https://docs.astral.sh/uv/reference/cli/#uv-tool).

Kaynak ZIP indirilirse kullanıcı arşivi açıp pyproject.toml bulunan kökte aynı yerel kurulum komutunu çalıştırabilir; Git sırf ZIP'ten kurmak için şart değildir. Sadece CLI kullanacak kişinin Node, npm, Android Studio veya API sunucusu kurması gerekmez. Ağlı analizin internet erişimi gerekir; offline profil kurulumdan sonra bağlantısız çalışabilir.

Geliştirici akışı, paketlenmiş kullanıcı kurulumundan ayrılır. Depo ve kilit dosyası oluştuktan sonra:

    uv sync --locked
    uv run sweb --offline -d www.example.com

API geliştirilirken api ek bağımlılıkları uv sync --locked --extra api ile alınır. Mobil npm komutları yalnızca apps/mobile içinde çalışır. uv run kaynak çalışma ortamını kullanır; daha önce uv tool install ile kurulmuş sweb, kaynak değişikliklerini otomatik izleyen bir editable kurulum sayılmaz.

Kullanıcı kurulumu ile geliştirici kilidi aynı garanti değildir: uv tool install yerel paketin ilan ettiği bağımlılıkları çözer; deponun uv.lock dosyasını kendiliğinden tükettiği varsayılmaz. Geliştirme/CI kilitli ortam kullanır; ayrıca temiz araç kurulumu desteklenen bağımlılık aralıklarıyla test edilir.

Yayınlarda wheel ve kaynak dağıtımı, sürüm etiketi, changelog ve kurulum/kaldırma yönergeleri bulunacak. Komut sweb, önerilen Python paket adı saferweb olarak düşünülüyor; PyPI adı ve komut çakışması yayın öncesi kontrol edilecek. Paket sahipliği doğrulanmadan uv tool install saferweb komutu README'ye yazılmayacak.

**Kendi geliştireceğimiz analiz bileşenleri**

| Bileşen | Tasarlanacak davranış | Yanlış çıkarımdan kaçınma |
| --- | --- | --- |
| Girdi sınıflandırma | URL, düz metin, Wi-Fi ve diğer QR içeriklerini ayırma; boyut ve karakter sınırları. | URL olmayan her QR kötü amaçlı sayılmaz. |
| URL doğrulama | Şema, host, port, kullanıcı bilgisi, yol, sorgu ve parçayı ayırma; belirsiz girdiyi reddetme. | Bir ayrıştırıcının girdiyi kabul etmesi, girdinin güvenli olduğu anlamına gelmez. |
| Alan adı çözümleme | Public Suffix List ve IDNA ile doğru alan adı sınırları; alt alan adıyla yanıltmayı değerlendirme. | Son iki etiketi almak com.tr gibi son eklerde doğru değildir. |
| Marka benzerliği | Düzenleme mesafesi, benzer Unicode karakterleri, marka adının host/path içindeki yeri; sürümlü marka-alan adı eşleşmeleri. | Benzerlik tek başına kimlik avı kararı vermemeli. |
| URL yapısı | Aşırı uzunluk, çok sayıda alt alan adı, IP kullanımı, yoğun kodlama, anormal karakter dağılımı. | Meşru imzalı ve takip URL'leri de uzun veya karmaşık olabilir. |
| Yönlendirme | Her adımın hostu, alan adı değişimi, döngü ve son hedef; her adımda ağ güvenliği doğrulaması. | URL kısaltıcı kullanımı kendi başına zararlılık kanıtı değildir. |
| DNS/TLS bağlamı | DNS yanıtları, TLS doğrulama sonucu, HTTPS'ten HTTP'ye geçiş; isteğe bağlı RDAP kayıt bilgileri. | HTTPS güvenilir içerik garantisi vermez; DNS/TLS hatası kimlik avı kanıtı değildir. |
| Statik HTML | Form alanları ve gönderim hedefleri, başlık/marka/hedef uyuşmazlığı, meta yönlendirme ve indirme işaretleri. | SSO ve ödeme akışlarında farklı alan adına form gönderimi meşru olabilir. |
| İndirme işaretleri | URL son eki, Content-Type ve Content-Disposition bağlamı. | APK/EXE uzantısı veya bir başlık dosyanın zararlı olduğunu doğrulamaz. |
| Kanıt ve skor | Bulgulara kural kimliği, açıklama, önem, kaynak, zaman ve motor sürümü ekleme. | Skor kalibre edilmedikçe olasılık veya yüzde güven olarak sunulmaz. |

Kendi algoritmamız; hangi özellikleri çıkaracağımız, bunları hangi bağlamda değerlendireceğimiz, benzerlik ölçümleri, kural birleşimleri ve karar gerekçeleridir. URL ayrıştırma, TLS, Unicode verisi ve HTML ayrıştırıcılarını güvenilir kütüphanelerden kullanacağız.

Örnek: https://banka.com.oturum.example.org/giris adresinde banka.com yalnızca alt alan adı metnidir; bağlantının kaydedilebilir alan adı example.org olur. Bu örnek sentetiktir ve canlı hedef olarak kullanılmaz. Böyle bir örnek, marka metni ile gerçek hedef arasındaki farkı kullanıcıya açıkça gösterir.

Normalleştirmede URL anlamı korunacak: yol ve sorgu bütünüyle küçük harfe çevrilmeyecek; bütün URL üzerinde tekrar tekrar yüzde çözme yapılmayacak. Analiz için çözümlenen temsiller ile gerçekten istenen adres ayrı tutulacak. JavaScript/WHATWG, Python ve HTTP istemcisinin aynı girdiyi farklı yorumlayabildiği durumlar test edilecek; belirsiz hedeflere istek atılmayacak. [WHATWG URL standardı](https://url.spec.whatwg.org/), [Python URL ayrıştırma güvenliği](https://docs.python.org/3/library/urllib.parse.html).

Alan adı değerlendirmesinde PSL'nin özel barındırma son ekleri de dikkate alınacak; bir github.io kiracısı tüm platformu temsil etmez. PSL bir güvenilir site listesi değildir. tldextract kendiliğinden bir URL doğrulayıcısı sayılmamalı; kullandığı PSL anlık taramada rastgele güncellenmek yerine sürümü belli bir yerel kopyadan alınmalı. [PSL açıklaması](https://publicsuffix.org/learn/), [tldextract belgeleri](https://github.com/john-kurkowski/tldextract).

Unicode/IDN kullanımı doğrudan tehdit sayılmayacak. Punycode ve Unicode gösterimleri, karışık alfabe ve bilinen marka benzerliği birlikte değerlendirilecek. Türkçe karakter içeren meşru alan adları test kümesine dahil edilecek. [Unicode güvenlik mekanizmaları](https://www.unicode.org/reports/tr39/).

**Motorun araç kutusu**

| Paket veya araç | Kullanımı | Aşama |
| --- | --- | --- |
| Python standart kütüphanesi | URL, IP, TLS, JSON ve temel veri işlemleri. | Çekirdek |
| Pydantic | CLI/API için ortak veri sözleşmesi. | Çekirdek |
| Click | CLI parametreleri, yardım ve hata akışı. | CLI |
| Rich | İnsan için terminal çıktısı; JSON yolu ayrıdır. | CLI |
| idna, tldextract | Host gösterimi ve PSL sınırları. | Çekirdek |
| HTTPX | Doğrulanan hedefe kısıtlı HTTP taşıma. | Ağ analizi |
| dnspython | DNS yanıtları ve adres kontrolü. | Ağ analizi |
| beautifulsoup4 | Sınırlı statik HTML'den form/bağlantı/meta çıkarımı. | İçerik analizi |
| RapidFuzz veya küçük referans uygulama | Marka benzerliği deneyleri. | Benzerlik kuralları |
| pytest / Hypothesis | Kural, CLI, parser ve güvenlik testleri. | Baştan itibaren |
| Ruff | Python biçim ve statik kontroller. | Baştan itibaren |
| FastAPI / Uvicorn | Mobil API; isteğe bağlı api bağımlılıkları. | Mobil entegrasyon |
| jest-expo / React Native Testing Library | İzin reddi, eksik sonuç ve link açma gibi önemli akışlar. | Mobil |
| scikit-learn | Kural motoruna karşı değerlendirilecek basit modeller. | Sonraki araştırma |

Paketler uygun proje ortamına kurulacak. HTTPX tek başına SSRF koruması sağlamaz. İstemci zaman aşımına ek olarak DNS ve yönlendirmeler dahil bütün işi sınırlayan süre bütçesi gerekir. [HTTPX zaman aşımı modeli](https://www.python-httpx.org/advanced/timeouts/).

**Risk raporunun tasarımı**

Her rapor; analiz edilen hedef, seçilen profil, yönlendirme adımları, bulgular, tamamlanan/atlanmış kontroller, zaman ve motor/kural sürümü taşımalı. Terminal ve mobil ekran anlaşılır gerekçeler gösterir; JSON aynı verinin sürümlü makine sözleşmesidir.

İlk sürüm açıklanabilir kural tabanlı yaklaşım kullanacak. Aynı olgudan türeyen ilişkili işaretler ayrı ayrı toplanarak yapay puan üretmeyecek. Bulgular kategori bazında gruplanacak; grup tavanları ve birleşimler doğrulama verisiyle ayarlanacak. Geçerli HTTPS sertifikası güçlü bir phishing bulgusunu sıfırlamayacak.

Risk ve analiz kapsamı ayrı eksenlerdir:

| Boyut | Örnekler |
| --- | --- |
| Risk değerlendirmesi | İncelenen kontrollerde belirgin risk işareti bulunmadı; şüpheli; yüksek risk; yeterli kanıt yok. |
| Analiz durumu | İstenen profil tamamlandı; kısmi; başarısız; politika gereği erişim engellendi. |
| Profil | URL metniyle sınırlı offline profil; DNS/TLS/HTTP içeren extended profil. |
| Kontrol kapsamı | Tamamlandı, uygulanamaz, profil dışında, zaman aşımı, erişim engeli. |

Offline profil başarılı şekilde tamamlanabilir; ağ kontrolleri profil dışında olarak gösterilir. Bu rapor site içeriği incelenmiş gibi sunulmaz. Extended profil bir aşamada durursa tamamlanmış bulgular saklanır; eksik veri sıfır risk gibi işlenmez. Sayısal skor kullanılırsa kalibrasyon olmadan tehdit olasılığı veya yüzde güven olarak adlandırılmaz.

**Tarayıcının kendi güvenliği**

Kullanıcı URL'sine backend üzerinden erişmek SSRF riski doğurur: saldırgan analiz servisini iç ağa veya bulut kimlik servisine eriştirebilir. Yerel CLI'de de aynı hedef denetimleri kullanıcının yerel ağını korumak için uygulanacak. Uygulama doğrulaması ve sunucu dağıtımında ağ kısıtları birlikte gerekir. [OWASP SSRF rehberi](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html).

Ortak tasarım kararları:

1. Ağ modülü yalnızca HTTP/HTTPS ve ilk sürümde 80/443 portlarına erişir. CLI/API keyfi cookie, Authorization header, HTTP yöntemi veya proxy aktarma arayüzü sağlamaz.
2. Loopback, özel ağ, link-local, metadata, multicast ve diğer genel internete ait olmayan hedefler IPv4 ve IPv6 için engellenir. Sunucunun kendi yönetim/API altyapısı ayrıca korunur.
3. DNS cevabı doğrulandıktan sonra bağlantı aynı doğrulanmış IP'ye kurulur; HTTP istemcisinin yeniden DNS çözerek kontrolü boşa çıkarması önlenir. HTTPS host/SNI ve sertifika doğrulaması doğru alan adıyla korunur.
4. Yönlendirmeler otomatik/sınırsız izlenmez; her hedef yeniden doğrulanır. Origin değişiminde kimlik bilgisi aktarılmaz. DNS/bağlantı katmanında kontrol gereksinimine örnek: [aiohttp SSRF açıklaması](https://docs.aiohttp.org/en/stable/client_middleware_cookbook.html#server-side-request-forgery-protection).
5. İlk mühendislik sınırları: en fazla 5 yönlendirme, işin tamamı için yaklaşık 10 saniye ve dekomprese edilmiş gövde için 1 MiB. Süre DNS'i de kapsar. Bu değerler ölçümle ayarlanacak.
6. Yavaş, sonsuz, aşırı büyük veya sıkıştırılarak büyütülmüş yanıtlar sınırlandırılır. Hedef başına istek ve toplam eşzamanlılık denetlenir; tekrar denemeler bütçeyi aşamaz.
7. Hedefin JavaScript'i çalıştırılmaz; form gönderilmez; indirilen içerik çalıştırılmaz. Yalnızca sınırlı statik HTML ayrıştırılır.
8. Hedef URL shell komutuna birleştirilmez; ağ erişimi Python kütüphaneleriyle yapılır. Shell injection ve terminale kontrol karakteri enjeksiyonu test edilir.
9. Ağ istemcisi ortamdan gelen proxy/kimlik bilgilerini kontrolsüz devralmaz. PSL ve diğer listeler tarama sırasında gizli bir ağ çağrısı üretmez; güncelleme işlemleri ayrı olur.
10. Sunucu ağ işçisi root yetkisi, Docker soketi, kullanıcı ev dizini veya üretim sırları taşımaz; çıkış ağı kısıtlıdır. CLI normal kullanıcı yetkisiyle başlar ve tam sandbox iddiası taşımaz. [Docker güvenlik modeli](https://docs.docker.com/engine/security/).
11. Ham web içeriği mobil WebView veya terminal markup'ı olarak gösterilmez; güvenilmeyen metin güvenli gösterime çevrilir.
12. Canlı hedeflere geçmeden önce DNS değişimi, yönlendirme ve kaynak limitleri kontrollü yerel laboratuvarda doğrulanır. Yerel fixture erişimi yalnızca test yapılandırmasına ait olur; yayınlanan CLI'ye güvenlik denetimini kaldıran bayrak eklenmez.

Emülatörden geliştirme API'sine erişim, yapılandırılmış birinci taraf bağlantıdır. Bu bağlantı için kullanılan yerel adresin çalışması, taranan URL'lere yönelik özel ağ yasağının kaldırılması anlamına gelmez.

**Gizlilik ve ürün sınırları**

Yerel CLI, URL'yi varsayılan olarak SaferWEB'e veya itibar servisine yüklemez. Extended profilde hedef sunucu ve kullanılan DNS çözümleyicisi, kullanıcının bağlantısı üzerinden gerçekleşen ilgili istekleri görebilir. Offline profil hiçbir ağ isteği yapmaz. Bu fark yardım ve README'de açıklanacak.

Tam URL şifre sıfırlama, tek kullanımlık giriş, davet veya ödeme belirteci taşıyabilir. Basit GET isteği bile böyle bir bağlantıyı tüketebilir veya izleme kaydı oluşturabilir. CLI'deki -d ile alan adı analizi ve mobildeki alan adıyla sınırlı seçenek, yol/içerik kapsamının azalacağını göstererek sunulacak. URL'den hassas bileşenleri çıkarıp aynı hedef bütünüyle incelenmiş gibi sonuç verilmeyecek.

CLI varsayılan olarak kalıcı geçmiş tutmaz. Kullanıcı JSON çıktısını dosyaya yönlendirirse bu onun seçtiği rapordur; hassas parametreler varsayılan çıktıda maskelenir. --stdin terminal geçmişine hassas URL yazma ihtiyacını azaltır. İlerleme ve hata logları da aynı maskeleme politikasını kullanır.

Mobilde QR görüntüsü cihazda kalır; kamera izni tarama ekranında istenir. Yalnızca QR okuyan kendi derlememizde mikrofon/kayıt izni gereksinimi kaldırılır. [Expo Camera izinleri](https://docs.expo.dev/versions/latest/sdk/camera/).

Mobil analiz için gönderilen URL backend tarafından görülebilir. Ham URL/sorgu varsayılan sunucu loglarında saklanmaz; geçmiş isteğe bağlı ve cihazda tutulur. API anahtarları mobil pakete gömülmez. Harici servise kullanıcı URL'si göndermek veya tehdit raporu yayımlamak ayrı, açıklanan özelliklerdir; otomatik gönderim varsayılanı yoktur.

**Harici veri ve kendi motorumuz**

Harici veri kullanmak kendi algoritmamızı geliştirmeye engel değildir. İlk çekirdek ağsız sentetik örneklerle çalışır. Sonraki veri kaynakları, etiketli değerlendirme malzemesi veya ayrı bir itibar bulgusu sağlar.

| Kaynak | Kullanım | Sınır |
| --- | --- | --- |
| OpenPhish | Phishing URL örnekleri veya isteğe bağlı besleme adaptörü. | Erişim planının kapsamı ve kullanım koşulları kontrol edilir. |
| URLhaus | Zararlı yazılım dağıtımıyla ilişkilendirilmiş URL verisi. | Phishing veri kümesinin yerine geçmez; API erişimi Auth-Key gerektirir. |
| Tranco | Çeşitli meşru örnekler bulmak için popüler alan adı adayları. | Popülerlik temiz içerik garantisi değildir; tek başına güvenilir negatif etiket üretmez. |
| Kontrollü sentetik örnekler | Ayrıştırma, yönlendirme, form ve güvenlik değişmezleri. | Gerçek dünyadaki tespit oranını tek başına kanıtlamaz. |

Kaynak kapsamı ve güncel erişim koşulları: [OpenPhish](https://openphish.com/phishing_feeds.html), [URLhaus](https://urlhaus.abuse.ch/api/), [Tranco](https://tranco-list.eu/).

Başlangıçta canlı zararlı siteler ziyaret edilmeden URL metinleri ve yerel zararsız HTML örnekleriyle çalışılacak. Veri kaynağı, zaman damgası, etiketin gerekçesi, kullanım/yeniden dağıtım koşulu ve dosya karması tutulacak. Kodun açık kaynak olması dış veri kümelerinin lisansını değiştirmez; repo içinde veri edinme yöntemi ve izin verilen örnekler bulunacak.

**Başarıyı nasıl ölçeceğiz?**

İlk güvenlik testi paketi en az 50 dikkatle seçilmiş URL/QR kenar durumu içerecek. Örnek grupları: meşru Türkçe/IDN alan adları, uzun imzalı URL'ler, kullanıcı bilgisi içeren URL'ler, yanıltıcı alt alan adları, URL kısaltıcılar, özel ağ hedefleri, IPv6, bozuk yüzde kodlamaları ve desteklenmeyen QR içerikleri.

Ağ laboratuvarı; yönlendirme ile özel ağa geçiş, DNS cevabının değişmesi, çok büyük/yavaş/sıkıştırılmış yanıt, yönlendirme döngüsü, farklı parser yorumları ve izin verilen hedef IP'den sapma testlerini içerecek. Ağ testleri gerçek kötü amaçlı örneklere veya üçüncü taraf API anahtarlarına bağımlı olmayacak.

CLI ve entegrasyon kabul testleri de eklenecek:

- sweb -Xd ile sweb -X -d aynı ayarlara ve bulgulara dönüşmeli; Fish ve Bash davranışları doğrulanmalı.
- Kurulu komut depo dışındaki bir dizinden çalışmalı; kurulum/kaldırma temiz Python ortamında denenmeli.
- Node, Android SDK ve FastAPI olmadan CLI kurulabilmeli ve çalışmalı.
- --offline sırasında DNS, HTTP, telemetri ve liste güncellemesi dahil ağ çıkışı olmamalı.
- --json stdout'u yalnızca geçerli JSON içermeli; stderr ve çıkış kodu sözleşmesi korunmalı.
- Karmaşık URL, tırnak, Unicode, ANSI/OSC ve Ctrl+C durumları komut veya terminal enjeksiyonu üretmemeli.
- Aynı sabit gözlemler ve motor sürümüyle CLI ve API raporları aynı bulguları vermeli.
- Android emülatöründe ağ kesilmesi, izin reddi, yinelenen QR ve eksik sonuç akışları denenmeli.
- Kuralları ölçen testler harici itibar eşleşmesini kapatabilmeli; CLI-only ve API kurulumları bağımsız denenmeli.

Tespit ölçümleri:

| Ölçüm | Anlamı |
| --- | --- |
| Precision | Riskli dediğimiz bağlantıların ne kadarı gerçekten etiketli tehdit? |
| Recall | Etiketli tehditlerin ne kadarını yakaladık? |
| Yanlış pozitif oranı | Temiz kabul edilen test örneklerinin ne kadarına yanlış alarm verdik? |
| Karışıklık matrisi | Doğru/yanlış olumlu ve olumsuz kararların sayıları. |
| Analiz kapsamı | Kaç girdide hangi kontroller tamamlandı; kaçında sonuç belirsiz kaldı? |
| Gecikme | Ortanca ve p95 süre; zaman aşımı oranı. |
| Katkı karşılaştırması | Hangi kural grubunu kapatınca sonuç değişiyor? |

Kuralları geliştirirken bile geliştirme, ayarlama ve final test kümeleri ayrılacak. Aynı veya çok benzer URL'ler, aynı alan adı ve mümkünse aynı kampanya farklı kümelere sızmayacak. Daha sonraki tarihli örneklerle ayrıca test yapılacak. Gruplu ayırma ile zamansal ayırma farklı gereksinimlerdir; standart rastgele bölme ikisini birden çözmez. [scikit-learn değerlendirme ve veri ayırma rehberi](https://scikit-learn.org/stable/modules/cross_validation.html).

Üç deney ayrı raporlanacak: yalnız kendi kurallarımız; yalnız harici itibar eşleşmesi; ikisinin birleşimi. Etiketi sağlayan beslemeyi aynı örneklerin testinde karar kaynağı olarak kullanıp bunu motor başarısı diye sunmayacağız. Geçmişte zararlı etiketlenmiş bir bağlantının bugünkü DNS/HTML durumu geçmiş etiketle otomatik olarak eşdeğer kabul edilmeyecek.

Sonuçlar ölçülene kadar yüzde başarı iddiası yayımlanmayacak. Sentetik test başarısı ile gerçek veri üzerindeki tespit başarısı ayrı raporlanacak. Belirsiz sonuçları metriklerin dışında görünmez biçimde bırakmak yerine oranları ve hesaplama politikası açıklanacak.

**CachyOS ortamını hazırlama**

Bu komutlar kullanıcının kendi CachyOS terminalinde uygun aşamada çalıştırılması içindir. Bu revizyon sırasında kurulum yapılmadı. Node v26.8.1, npm 12.0.2 ve varsayılan Fish bilgileri zaten alındı; yeniden sorulmayacak.

CLI geliştirmesi için ilk araçlar:

    sudo pacman -Syu --needed git uv curl
    uv python install 3.13

Doğrulama:

    git --version
    uv --version
    uv python find 3.13

İlk komut tam sistem güncellemesi ve gereken paketleri kurar. uv tarafından yönetilen proje Python'ı sistem Python'ının yerine geçirilmez. [Arch uv paketi](https://archlinux.org/packages/extra/x86_64/uv/), [uv Python yönetimi](https://docs.astral.sh/uv/guides/install-python/).

Node değişikliği CLI'yi geliştirmek için ön koşul değildir. Mobil ortam hazırlanırken:

    sudo pacman -Syu --needed fnm jdk17-openjdk

Fish yapılandırması için ~/.config/fish/conf.d/fnm.fish dosyasına, zaten bulunmuyorsa şu satır eklenir:

    fnm env --use-on-cd --shell fish | source

Aynı satır mevcut terminalde çalıştırılarak oturuma uygulanabilir. Ardından:

    fnm install 24
    fnm use 24
    node --version
    npm --version

Mobil proje kurulunca sürüm .node-version dosyasında seçilen Node 24 hattıyla kaydedilir. Npm 12.0.2 bu ortama zorla taşınmaz; seçilen Node ile gelen npm sürümü doğrulanır. Sistem Node'u/paketleri körlemesine kaldırılmaz. [Arch fnm paketi](https://archlinux.org/packages/extra/x86_64/fnm/), [fnm Fish kurulumu](https://github.com/Schniz/fnm#fish-shell).

VS Code başlangıç eklentileri:

| Eklenti | Yayıncı | Amaç |
| --- | --- | --- |
| Python / Pylance | Microsoft | Python ortamı ve tip desteği. |
| Ruff | Astral | Python biçim ve statik kontrol. |
| ESLint | Microsoft | Mobil JavaScript/TypeScript kuralları. |
| Prettier | Prettier | Mobil biçim düzeni. |

Geliştirici Python yorumlayıcısı proje .venv içinden seçilecek. uv.lock, mobil package-lock.json ve sürüm kararları repoda tutulacak. Normal kullanıcı CLI kurulumunu uv tool install ile yapacak; geliştirici kaynak değişiklikleri için uv run kullanacak. Sistem geneline Python uygulama bağımlılıkları kurmak gerekmiyor.

Expo CLI mobil projenin expo paketinden npx ile kullanılır; ayrı eski global expo-cli kurulumu yoktur. [Expo CLI](https://docs.expo.dev/more/expo-cli/).

**Mobil çalışma ortamı**

Kullanıcının seçimi doğrultusunda ana test cihazı Android Studio içindeki Android Emulator/AVD olacak. Kod VS Code'da yazılacak. Android Studio, SDK Manager ve Device Manager işlerini üstlenecek. Emülatör ekranında çalışan uygulama Android uygulamasıdır; masaüstü CLI aynı anda normal Linux terminalinde çalışır.

Kurulum sırası:

1. Android Studio'nun resmi Linux sürümünü indirip kur.
2. SDK Manager'da seçilen Expo SDK'nın istediği Android SDK Platform, Build-Tools, Platform-Tools, Command-line Tools ve Emulator bileşenlerini yükle.
3. Güncel Expo rehberi JDK 17 ve API 36'yı gösteriyor; proje yaratılınca SDK/Gradle beklentileriyle eşleştir. Android Studio'nun kendi Java çalışma zamanı ile projeyi derleyen JDK ayarı birbirinden ayrıdır.
4. SDK Manager'daki gerçek Android SDK Location konumunu kaydet. Fish ortamına yalnızca bu doğrulanmış konumu ekle.
5. Device Manager'da başlangıç için tek bir uygun x86_64 telefon AVD'si oluştur; Intel/AMD x86_64 makinede bu mimariyi tercih et. Farklı ana makine mimarisi varsa imaj onunla eşleştirilir.
6. KVM hızlandırmasının kullanılabilir olduğunu doğrula; CPU sanallaştırması ve /dev/kvm erişimi gerektiğinde ayrıca düzeltilir.
7. İlk arayüz denemesi emülatörde Expo Go ile yapılabilir. Projenin kendi izinleri, yerel yapılandırması ve paket davranışı development build ile doğrulanır.
8. Kamera testleri için AVD'nin desteklediği sanal sahneye sentetik QR görselleri ekle veya desteklenen kamera kaynağını kullan. QR'den çıkarılmış metin testleri de motora bağımsız girdi sağlar.
9. Kamera modülünün seçilen emülatör/imaj kombinasyonunda çalışması bir kabul kontrolüdür. Gerçek kamera odağı/ışık koşulları emülatörle kanıtlanmış sayılmaz; donanım testi eksikliği sürüm test notlarında belirtilir.

[Android Studio kurulumu](https://developer.android.com/studio/install), [Expo Android ortamı](https://docs.expo.dev/workflow/android-studio-emulator/), [emülatör kamera kontrolleri](https://developer.android.com/studio/run/emulator-extended-controls).

SDK konumu gerçekten ~/Android/Sdk ise Fish için örnek ortam ayarı:

    set -Ux ANDROID_HOME $HOME/Android/Sdk
    set -Ux JAVA_HOME /usr/lib/jvm/java-17-openjdk
    fish_add_path $ANDROID_HOME/platform-tools $ANDROID_HOME/emulator

Bu yollar SDK Manager ve kurulu JDK konumu ile eşleşmelidir. Başka konum varsa örnek aynen uygulanmaz. SDK içindeki Platform-Tools adb sağlar; emülatör için ayrıca android-tools ve USB udev kuralları kurmak şart değildir.

Doğrulama komutları:

    java -version
    adb version
    emulator -accel-check
    emulator -list-avds
    adb devices

Linux Android Emulator KVM kullanır; hızlandırma -accel-check ile kontrol edilebilir. CachyOS'ta Ubuntu'nun apt kurulum komutları kullanılmayacak; çalışan emülatör için ayrıca VirtualBox veya tam bir libvirt ortamı kurmak varsayılan gereksinim değildir. [Android Emulator hızlandırması](https://developer.android.com/studio/run/emulator-acceleration).

Emülatörün localhost'u emülatörün kendisidir. Mobil API bağlantısı, belgelenmiş host erişimi veya ADB port yönlendirmesiyle kurulup küçük bir bağlantı testiyle doğrulanır. API ve Metro geliştirme bağlantıları ayrı uçlardır. Yerel API için HTTP gerekiyorsa yalnızca geliştirme yapılandırmasına ait olur; yayınlanan mobil uygulama güvenli HTTPS uç noktasını kullanır. [Android Emulator ağı](https://developer.android.com/studio/run/emulator-networking).

İlk amaç emülatörün açılması ve geliştirme uygulamasının çalışmasıdır. QR kamera testindeki donanım sınırlamaları CLI/motor geliştirmesini durdurmaz.

**Ağ analizi ortamı**

CLI'nin normal kurulumunda API servisi veya Docker zorunlu olmayacak. Yerel ağ modülü aynı URL/IP/DNS/yönlendirme denetimleriyle, normal kullanıcı yetkisinde ve kaynak sınırlarıyla çalışır. Offline profil için hiçbir ağ bileşeni etkinleşmez.

Güvenli ağ erişimini geliştirip sınarken kontrollü HTTP/DNS fixture'ları gerekir. Docker Engine + Compose bu laboratuvarı ve sonraki API işçisini tekrar üretilebilir tutmak için kullanılacak:

    sudo pacman -Syu --needed docker docker-compose

Bu yalnızca paket kurulumudur. Servis/erişim modeli, kaynak limitleri ve çıkış ağı politikası ayrıca yapılandırılır. Docker Desktop şart değildir. [Arch Docker](https://archlinux.org/packages/extra/x86_64/docker/), [Arch Compose](https://archlinux.org/packages/extra/x86_64/docker-compose/).

Kendi mobil API trafiği için curl ve FastAPI doküman arayüzü başlangıçta yeterli. İhtiyaç doğduğunda Burp Suite Community; paket düzeyinde sorun varsa Wireshark eklenebilir. Bunlar analiz karar motorunu oluşturmaz.

İlk aşamalara PostgreSQL, Redis/Celery, Kubernetes, Kali veya dosya çalıştıran malware laboratuvarı eklenmeyecek. CLI ile mobil ürün için gerekli olan işlere göre bağımlılıklar açılır. Sonradan uzun süren/kalıcı sunucu işleri oluşursa kuyruk ve veritabanı tasarlanır.

**Aşamalı geliştirme planı**

Geliştirme sırası CLI'yi erken teslim edecek şekilde değiştirildi; mobil uygulama proje kapsamının parçası olmaya devam ediyor.

| Aşama | Yapılacak iş | Bitiş ölçütü | Tahmini odaklı süre |
| --- | --- | --- | --- |
| 0 | Git/uv/Python, Fish ve repo kararları; mobil ortam listesi | CLI geliştirme araçları doğrulanmış; Node/npm/Fish bilgileri kayda alınmış. | 6–10 saat |
| 1 | Tehdit modeli, CLI bayrakları, ortak sonuç şeması, örnekler | En az 50 seçilmiş kenar durumu; sweb sözleşmesi ve beklenen sonuçlar yazılı. | 8–12 saat |
| 2 | Ağsız ortak motor ve açıklanabilir kurallar | API anahtarı olmadan URL bulguları üretilebiliyor; testler geçiyor. | 18–26 saat |
| 3 | Click/Rich CLI, paketleme, JSON ve çıkış kodları | GitHub'dan alınan kaynak kuruluyor; sweb depo dışından Fish/Bash'te çalışıyor. | 14–22 saat |
| 4 | Güvenli ağ modülü, -X ve kontrollü laboratuvar | DNS/IP/yönlendirme/limit testleri geçiyor; sweb -Xd ile gerçek ağ analizi yapılabiliyor. | 26–38 saat |
| 5 | Statik HTML ve bağımsız değerlendirme | Kural katkısı, yanlış alarm, gecikme ve kapsam raporu tekrar üretilebiliyor. | 24–36 saat |
| 6 | FastAPI, Android Studio/AVD, mobil URL akışı | Emülatördeki mobil uygulama aynı motorun gerçek raporunu gösteriyor. | 24–36 saat |
| 7 | QR/izinler, ürün testleri, CI ve açık kaynak yayın | CLI dağıtımı ve Android derlemesi belgeli; demo ve sınırlar yayıma hazır. | 16–24 saat |

Toplam yaklaşık 140–205 odaklı saat. Haftada 10–15 saatle yaklaşık 10–21 haftalık planlama aralığı düşünülebilir. Framework öğrenme, emülatör sorunları ve güvenli ağ erişimi süreyi uzatabilir. İlk kurulabilir offline CLI, mobil uygulamadan ve bütün özelliklerden önce teslim edilir.

Kilometre taşları:

| Kilometre taşı | Gösterilebilir sonuç |
| --- | --- |
| İlk CLI demosu | Kurulabilen sweb, --help, ağsız URL analizi ve açıklanabilir rapor. |
| Ağlı CLI | Kullanıcının sweb -Xd www.example.com hedefi; kısıtlı DNS/TLS/HTTP ve JSON çıktısı. |
| Mobil entegrasyon | Android emülatöründe aynı motor üzerinden URL analizi. |
| İlk bütünleşik yayın | CLI + Android URL/QR akışı + ölçüm raporu + kurulum/katkı belgeleri. |

Öğrenme sırası: URL/domain/DNS/TLS temelleri; Python paket/test düzeni; Click ve shell argümanları; özellik çıkarımı; SSRF ve ağ katmanı; ölçüm; FastAPI; React Native/Expo ve Android emülatörü. Bash burada CLI'yi kullanıp otomasyona bağlamak için öğrenilir; güvenlik kararlarının ikinci bir Bash uygulaması yazılmaz.

**GitHub yapısı ve yayın kalitesi**

Tek depolu, tek Python dağıtımı içeren başlangıç yapısı:

| Yol | İçerik |
| --- | --- |
| pyproject.toml | Python paket bilgileri, build tanımı, sweb entry point ve isteğe bağlı api bağımlılıkları. |
| uv.lock | Geliştirici/CI bağımlılık kilidi. |
| src/saferweb/core/ | Ortak modeller, doğrulama, özellikler, kurallar ve risk raporu. |
| src/saferweb/cli/ | Click seçenekleri, terminal/JSON çıktı ve çıkış kodları. |
| src/saferweb/network/ | DNS, hedef denetimi, HTTP taşıma ve toplama sınırları. |
| src/saferweb/api/ | FastAPI arayüzü ve işçi koordinasyonu; core CLI'ye bağımlı olmaz. |
| apps/mobile/ | Expo/TypeScript mobil uygulama ve kendi npm kilidi. |
| tests/ | Çekirdek, CLI, API, ağ politikası ve parity testleri. |
| tests/fixtures/ | Zararsız URL, HTML ve yerel ağ senaryoları. |
| evaluation/ | Veri edinme notları, deney tanımları ve sonuçlar. |
| infra/ | Docker/Compose ve kontrollü laboratuvar yapılandırması. |
| docs/ | Ana plan, tehdit modeli, CLI sözleşmesi ve mimari kararlar. |

Bu düzen önceki ayrı Python paket dizinleri önerisinin yerini alır. Motor mantıksal olarak bağımsız kalır; ilk aşamada aynı depodan kurulumu zorlaştıracak birden fazla Python dağıtımı oluşturulmaz. API/mobil katmanı core'un kendi arayüzü üzerinden çalışır.

README'de CLI kurulumu, sweb -Xd örneği, offline/extended ayrımı, terminal ekran görüntüsü, mobil demo ve sınırlamalar bulunur. CLI kullanıcısına mobil geliştirme bağımlılıkları yükleten tek bir dev kurulum akışı sunulmaz.

LICENSE için öneri MIT; dış veri ve bağımlılık koşulları ayrıca korunur. SECURITY.md açık bildirim yolunu; CONTRIBUTING.md katkı/test düzenini anlatır. CHANGELOG sürüm değişikliklerini ve CLI sözleşmesi değişikliklerini kaydeder. Bağımsız denetim yapılmadan denetlenmiş/güvenliği garanti edilmiş iddiaları yazılmaz.

CI'de Python lint/test, temiz wheel/CLI kurulumu, Fish/Bash komut kontrolleri, offline ağsızlık ve JSON/exit-code sözleşmesi çalışır. Mobil akışta TypeScript kontrolü ve önemli ekran testleri eklenir; emülatör testi uygun işte yürütülür. Fork PR'ları üretim sırları veya canlı zararlı örnekler gerektirmez.

Her kural değişikliği açıklama ve yeni örneklerle değerlendirilir. Gerçek kullanıcı URL geçmişi, sırlar, ham malware örnekleri ve yeniden dağıtılamayan veri repoya girmez. Release paketi doğru sweb komutunu, kendi sürümünü ve yerel kurallarını içerdiği doğrulanarak yayımlanır.

**İlk sürümden sonraki araştırma yolları**

Makine öğrenmesi: Basit bir lojistik regresyon veya ağaç modeli, kendi özelliklerimiz üzerinde kural tabanlı motorla karşılaştırılır. Model ancak bağımsız testte yarar gösteriyorsa ürüne eklenir. Gerekirse kalibrasyon ayrıca yapılır.

Dosya analizi: İndirilen APK/EXE'nin içeriği üzerinde ayrı bir iş akışı tasarlanır. Dosya türü ve karma, statik inceleme, YARA kuralları ve APK için Androguard gibi araçlar değerlendirilir. Statik eşleşme ile dinamik davranış analizi farklı kanıt türleridir. Bu araçların güncel sürüm, lisans ve izolasyon ihtiyaçları o aşamada doğrulanmalıdır.

Dinamik web analizi: JavaScript çalıştırmak gerekiyorsa tek kullanımlık, kimlik bilgisiz ve ağ çıkışı kısıtlı ayrı bir tarayıcı ortamı tasarlanır. Bu, başlangıç HTML ayrıştırmasının güvenlik maliyetini belirgin biçimde artırır.

Çevrimdışı analiz: Mobil cihaz üzerinde çalışacak küçük bir URL değerlendirme katmanı ve kuralların güncellenme/doğrulanma yöntemi araştırılır. İlk backend motoruyla davranış eşitliği ortak test örnekleriyle korunur.

Masaüstü GUI: Kullanıcı ileride pencereli uygulama isterse CLI/core üzerine ayrı bir arayüz değerlendirilebilir. Mevcut masaüstü teslimatı sweb terminal aracıdır; Electron/Tauri bağımlılığı bu revizyona eklenmedi.

**Kesinleşen bilgiler ve sıradaki ortam adımı**

Node v26.8.1, npm 12.0.2, varsayılan Fish ve Android Studio emülatörü seçimi kesinleşti. Kullanıcıya bunlar yeniden sorulmayacak.

Bir sonraki ortam oturumunda Git/uv/Python kurulumlarının durumu doğrulanacak. Mobil araçlara geçerken Android Studio SDK konumu ve KVM kullanılabilirliği ölçülecek. Proje oluşturulurken gerçek GitHub repo sahibi/adresi ve paket/komut adı çakışmaları kontrol edilecek; bunlar bu plan revizyonunun tamamlanmasını engellemez.

İlk ürün kabul hedefi: kullanıcı depoyu indirir, belgelenmiş kurulumu yapar, Fish veya Bash terminalinde sweb komutunu çalıştırır. Motor önce ağsız, ardından -X ile ağlı değerlendirme sunar. Android uygulama bu motoru FastAPI üzerinden kullanan ikinci arayüz olarak aynı geliştirme planında ilerler.
