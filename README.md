# 🔐 ChatBase

**ChatBase**, uçtan uca şifrelenmiş, tek seferlik ve anonim mesaj paylaşımı için geliştirilmiş bir Python projesidir.  
Mesajlar AES-256 ile şifrelenir, SQLite veritabanında saklanır ve geçici bir Flask + Ngrok sunucusu üzerinden güvenli şekilde paylaşılır.

> Yapımcı: **farmerdev**  
> Amaç: Güvenli, sade ve iz bırakmayan mesajlaşma

---

## ✨ Özellikler

- 🔒 **AES-256 (CBC) şifreleme**
- 🧠 **PBKDF2 ile anahtar türetme**
- 🗄️ **SQLite veritabanı**
- 🎭 **Sahte (fake) mesajlar ile veri gizleme**
- 🌐 **Flask tabanlı mini web arayüz**
- 🚀 **Ngrok ile dış dünyaya açma**
- 🔑 **Erişim şifresi koruması**
- 🛑 **IP Rate Limiting (3 hatalı deneme → 1 dk ban)**
- 🧹 **İndirme sonrası otomatik veritabanı silme (opsiyonel)**
- 🌍 **Türkçe / İngilizce web arayüzü**

---

## 🧩 Proje Yapısı
chatbase/
│
├── chatbase.py # Ana Python uygulaması
├── db/ # Şifreli SQLite veritabanları
├── server/
│ └── index.html # Web arayüzü
└── README.md


---

## 🔐 Güvenlik Mimarisi

### Şifreleme
- Mesajlar **AES-256-CBC** ile şifrelenir
- Her mesaj için:
  - Rastgele **salt**
  - Rastgele **IV**
- Anahtar türetme:
  - `PBKDF2-HMAC-SHA256`
  - 100.000 iterasyon

### Sahte Kayıtlar
Gerçek mesajın yanına **9 adet sahte şifreli kayıt** eklenir.  
Böylece:
- Hangi kaydın gerçek olduğu anlaşılamaz
- Bruteforce ve analiz zorlaşır

---

## 🚀 Çalışma Mantığı

### 1️⃣ Mesaj Gönderme
- Kullanıcı mesajı girer (çok satırlı destek)
- MASTER PASSWORD belirler (min. 8 karakter)
- Karşı taraf için erişim şifresi belirler
- Geçici veritabanı oluşturulur
- Flask sunucusu başlar
- Ngrok üzerinden paylaşım linki üretilir

### 2️⃣ Mesaj Alma
- `.db` dosyası alınır
- Aynı MASTER PASSWORD girilir
- Veritabanındaki gerçek mesaj çözülür ve gösterilir

---

## 🌐 Web Arayüzü

- Terminal temalı minimalist tasarım
- Şifre girişi ile indirme
- Yanlış girişlerde hata mesajı
- IP bazlı rate limit desteği
- Otomatik dosya indirme (`chatbase_message.db`)

---

## 🛠️ Kurulum

### Gereksinimler
```bash
pip install flask pycryptodome rich pyngrok

###Çalıştırma
python main.py

⚠️ Güvenlik Notları

MASTER PASSWORD asla kaydedilmez

Yanlış MASTER PASSWORD ile mesaj çözülemez

Ngrok linki kapandığında erişim sona erer

Proje kullanım amacı kullanıcıya aittir

Yasalara aykırı kullanımı ve sorumluluğu kullanıcınındır

Geliştirici kabul etmez
