# ELİZ AYAKKABI – Stok & Satış Yönetim Sistemi

Java (Swing) ve SQLite ile geliştirilmiş, **nesne yönelimli programlama (OOP)** prensiplerine
dayalı bir masaüstü **mağaza yönetim uygulamasıdır**. Bir ayakkabı mağazasının ürün, stok,
satış, personel ve kullanıcı işlemlerini tek bir arayüzden yönetmeyi amaçlar.

> Bu proje bir OOP ders/okul projesidir. Katmanlı mimari (Model – DAL – UI) örneği olarak hazırlanmıştır.

---

## 🎯 Proje Amacı

- Bir mağazanın **ürünlerini, stoklarını ve satışlarını** kayıt altına almak
- **Personel** ve **kullanıcı (yetkili)** bilgilerini yönetmek
- Yönetici ve personel için ayrı **giriş / yetkilendirme** ekranları sunmak
- Veritabanı işlemlerini (CRUD) tek bir soyut katman üzerinden yürüterek
  **nesne yönelimli tasarımı** (kalıtım, soyutlama, kapsülleme) uygulamak

---

## ✨ Özellikler

- 🔐 **Giriş ekranı:** Yönetici ve personel girişi
- 🔑 **Şifre işlemleri:** Şifre değiştirme, "şifremi unuttum" akışı
- 👟 **Ürün işlemleri:** Ürün ekleme / listeleme (marka, kategori vb.)
- 📦 **Stok işlemleri:** Stok takibi ve arşiv görüntüleme
- 🧾 **Satış işlemleri:** Satış ekranı ve satış-stok arşivi
- 👥 **Personel işlemleri:** Personel ekleme / güncelleme
- 🙍 **Kullanıcı (yetkili) yönetimi**

---

## 🏗️ Mimari ve Proje Yapısı

Uygulama üç katmana ayrılmıştır:

```
MyProject/
├── src/
│   ├── MyClass/          # Model (veri) sınıfları
│   │   ├── Kullanici.java
│   │   ├── Personel.java
│   │   ├── Product.java
│   │   └── Satis_Stok.java
│   │
│   ├── MyDataBase/       # Veri Erişim Katmanı (DAL) + veritabanı bağlantısı
│   │   ├── SqLiteConnect.java   # Soyut SQLite bağlantı/CRUD altyapısı
│   │   ├── IDAL.java            # DAL arayüzü
│   │   ├── KullaniciDAL.java
│   │   ├── LoginDAL.java
│   │   ├── PersonelDAL.java
│   │   ├── ProductDAL.java
│   │   ├── SatisDAL.java
│   │   └── StokDAL.java
│   │
│   └── MyFrame/          # Kullanıcı arayüzü (Java Swing ekranları)
│       ├── Main.java            # Uygulama giriş noktası
│       ├── Giris.java           # İlk giriş ekranı
│       ├── SifreGiris.java      # Yönetici şifre girişi
│       ├── AnaPanel.java        # Yönetici ana paneli
│       ├── UrunIslemleri.java
│       ├── StokIslemleri.java
│       ├── SatisEkrani.java
│       ├── Satis_Stok_arsiv.java
│       ├── PersonelIslemleri.java
│       ├── KullaniciView.java
│       ├── SifreDegis.java
│       └── SifreUnuttum.java
│
├── lib/
│   └── sqlite-jdbc-3.32.3.2.jar   # SQLite JDBC sürücüsü
├── *.db                 # SQLite veritabanı dosyaları (örnek veri)
├── nbproject/           # NetBeans proje ayarları
├── build.xml            # Ant derleme betiği
└── manifest.mf
```

**Katmanlı yapı:**

| Katman | Paket | Görevi |
|--------|-------|--------|
| **Model** | `MyClass` | Veriyi temsil eden nesneler (getter/setter) |
| **DAL** | `MyDataBase` | Veritabanı CRUD işlemleri; `SqLiteConnect` soyut sınıfından kalıtım |
| **UI** | `MyFrame` | Java Swing ekranları |

---

## 🛠️ Gereksinimler

- **Java JDK 8** (proje `javac.source`/`target = 1.8` ile derlenir)
- **SQLite JDBC sürücüsü** → `lib/sqlite-jdbc-3.32.3.2.jar` (repoya dahildir)
- Çalıştırma için: **NetBeans** (önerilir) veya **Apache Ant**, ya da elle `javac`/`java`

---

## 🚀 Kurulum ve Çalıştırma

### Yöntem 1 – NetBeans ile (önerilen)
1. NetBeans'te **File → Open Project** ile `MyProject` klasörünü açın.
2. SQLite sürücüsü `lib/` klasöründen göreli olarak referanslanır; ek ayar gerekmez.
3. **Run (F6)** ile çalıştırın. Giriş noktası: `MyFrame.Main`.

### Yöntem 2 – Apache Ant ile
```bash
cd MyProject
ant clean
ant jar
java -jar dist/MyProject.jar
```

### Yöntem 3 – Komut satırında elle derleme
```bash
cd MyProject
# Derle
javac -cp "lib/sqlite-jdbc-3.32.3.2.jar" -d build/classes src/MyClass/*.java src/MyDataBase/*.java src/MyFrame/*.java
# Çalıştır (Windows için ';', Linux/macOS için ':' ayırıcı)
java -cp "build/classes;lib/sqlite-jdbc-3.32.3.2.jar" MyFrame.Main
```

---

## 🗄️ Veritabanı

- Uygulama **SQLite** kullanır; her tablo için ayrı bir `.db` dosyası oluşturulur
  (`Personel.db`, `Satislar.db`, `Stok.db`, `UrunEkle.db`).
- Bağlantı `SqLiteConnect` sınıfında `jdbc:sqlite:<ad>.db` biçiminde kurulur.
- `.db` dosyaları çalışma dizininde oluşturulur/aranır.

> ⚠️ **Güvenlik notu:** Mevcut sürümde kullanıcı şifreleri veritabanında **düz metin**
> olarak saklanmaktadır. Gerçek kullanımda şifrelerin **hash**'lenmesi (ör. BCrypt) önerilir.

---

## 🔧 Bu Sürümde Yapılan Düzenlemeler

Projenin paylaşıma uygun hale getirilmesi için aşağıdaki temizlikler yapıldı:

- **Makineye özel mutlak yol kaldırıldı:** `nbproject/project.properties` içindeki
  `C:\Users\Elif\Desktop\sqlite-jdbc-...jar` referansı, taşınabilir olması için
  `lib/sqlite-jdbc-3.32.3.2.jar` göreli yoluna çevrildi.
- **Kullanıcıya özel NetBeans ayarları çıkarıldı:** `nbproject/private/` (mutlak yollar ve
  kullanıcı adı içeriyordu) ve yeniden üretilebilen `build/`, `dist/` klasörleri repodan hariç tutuldu.
- **Sabit kodlanmış kişisel bilgiler maskelendi:** Örnek kullanıcı adı ve telefon numarası
  placeholder değerlerle (`KULLANICI_ADI`, `XXX XXX XX XX`) değiştirildi.
- **Ölü/gereksiz kod temizlendi:** Kullanılmayan debug metotları (`queryTest`, `showList`),
  yorum satırına alınmış kod blokları ve kullanılmayan `import`'lar kaldırıldı.
- **`.gitignore` eklendi.**

> Not: İncelemede gerçek bir **API anahtarı / gizli token bulunmamıştır**; proje yerel bir
> SQLite uygulamasıdır. "Gizlenmesi gereken" tek öğeler yukarıdaki makineye özel yollar ve
> kişisel bilgilerdi.

---

## 📄 Lisans

Eğitim amaçlı bir projedir. Serbestçe incelenip geliştirilebilir.

## 👤 Geliştirici

**Elif Öksüzali** — [GitHub](https://github.com/elifoksuzali)
