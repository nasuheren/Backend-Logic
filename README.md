# Backend-Logic

Bu soruyu sorman **mükemmel bir başlangıç** 👏
Backend’i anlamak için önce **mantığını**, sonra **nasıl yazıldığını** öğrenmen gerekiyor — sadece kod yazmayı değil, “neden öyle yazıldığını” da bilmen lazım.
O yüzden sana **hiç bilmeyen biri gibi**, sıfırdan adım adım anlatacağım.
Bu, bir “yol haritası + mantık rehberi” olacak.
Hazırsan başlıyoruz 🚀

---

## 🧩 1. Backend Nedir (Basitçe)

Bir uygulamada **frontend (görünür kısım)** vardır, bir de **backend (arka planda çalışan kısım)**.

| Örnek                     | Frontend (Kullanıcıya görünen)  | Backend (Görünmeyen)                                     |
| ------------------------- | ------------------------------- | -------------------------------------------------------- |
| Berber Randevu Uygulaması | Kullanıcı ekranı, randevu formu | Randevuları kaydeden, doğrulayan sistem                  |
| Instagram                 | Gönderi ekranı, beğen butonu    | Veritabanına “beğeni” kaydını yapan API                  |
| Yemeksepeti               | Restoran listesi                | Siparişi alan, mutfağa ileten, ödemeyi doğrulayan sistem |

> Yani **backend**, uygulamanın beynidir 🧠
> Frontend göz, kulak, el gibidir; backend düşünür, karar verir ve kayıt tutar.

---

## 🧠 2. Backend Ne İş Yapar?

Backend’in ana görevi:

> “İstemciden (frontend) gelen istekleri al, işle, veritabanıyla konuş, sonuç döndür.”

Bunu 4 ana adımda yapar:

1. **İstek Alır** → “Kullanıcı kayıt olmak istiyor”
2. **Kontrol Eder** → “Bu e-posta zaten kayıtlı mı?”
3. **Veriyle Çalışır** → “Yeni kullanıcıyı veritabanına ekle”
4. **Cevap Döner** → “Kayıt başarılı ✅”

Bu mantık **her backend uygulamasında ortaktır.**

---

## ⚙️ 3. Backend Yazarken Kullanılan Parçalar

Bir backend uygulamasını yazarken genelde şu parçaları kullanırsın:

| Parça                       | Görev                                                        |
| --------------------------- | ------------------------------------------------------------ |
| **Sunucu (Server)**         | Gelen istekleri karşılayan uygulama                          |
| **API (Endpoint)**          | İstekleri belirli adreslere yönlendirir (`/users`, `/login`) |
| **Veritabanı (Database)**   | Verileri saklar (kullanıcılar, siparişler, randevular)       |
| **Model**                   | Veritabanındaki tabloların yapısını tanımlar                 |
| **Controller**              | İstekleri alır, uygun işlemi başlatır                        |
| **Service / İş Mantığı**    | Asıl hesaplamalar, kurallar, kontroller burada yapılır       |
| **Auth (Kimlik Doğrulama)** | Kullanıcının giriş yetkisini kontrol eder (JWT, session vb.) |

---

## 🧱 4. Backend’in Genel Akışı (Bir Hikâye Gibi Düşün)

Bir örnekle anlatalım:

### Örnek: Kullanıcı kayıt olmak istiyor

1. Kullanıcı uygulamada formu doldurur → **Frontend** → `/register` adresine veri gönderir.
2. **Backend** bu isteği alır (`POST /register`)
3. Controller der ki: “Tamam, bu kaydı `UserService` halletsin.”
4. `UserService` kontrol eder:

   * Bu e-posta zaten var mı?
   * Şifre geçerli mi?
5. Eğer her şey uygunsa:

   * Şifreyi güvenli hale getirir (`bcrypt` ile hash’ler)
   * Veritabanına ekler (`users` tablosuna kaydeder)
6. Son olarak bir cevap döner:

   ```json
   {
     "message": "Kayıt başarılı",
     "userId": 123
   }
   ```

---

## 🧩 5. Backend Geliştirme Süreci (Yol Haritası)

Şimdi gelelim “nasıl yazılır” kısmına:
Backend yazarken genelde şu adımlar izlenir:

### 🧭 1️⃣ Planlama

* Hangi özellikler olacak? (örnek: kullanıcı kaydı, randevu alma)
* Hangi roller var? (örnek: müşteri, berber, admin)
* Hangi veriler tutulacak? (örnek: kullanıcı adı, tarih, randevu saati)

🗒️ Örnek tablo:

| Koleksiyon / Tablo | Alanlar                          |
| ------------------ | -------------------------------- |
| users              | id, name, email, password, role  |
| appointments       | id, userId, barberId, date, time |

---

### ⚙️ 2️⃣ Proje Kurulumu

Bir backend framework seçersin:

* Başlangıç için → **NestJS** veya **Express.js**
* Kurulumdan sonra temel dosyaları oluşturursun:

  * `app.module.ts` (modüller)
  * `users.controller.ts` (istekler)
  * `users.service.ts` (iş mantığı)
  * `users.schema.ts` (veritabanı modeli)

---

### 🔗 3️⃣ Veritabanı Bağlantısı

Kullanılabilecek veritabanları:

| Tip   | Örnek             | Ne zaman kullanılır               |
| ----- | ----------------- | --------------------------------- |
| SQL   | PostgreSQL, MySQL | İlişkisel veriler (tablo tabanlı) |
| NoSQL | MongoDB           | Esnek veri yapıları (JSON tarzı)  |

NestJS örneği:

```ts
MongooseModule.forRoot('mongodb://localhost:27017/mydb')
```

---

### 🧮 4️⃣ API Endpoint’lerini Yaz

Her işlem için bir **endpoint (API adresi)** tanımlarsın:

| İşlem           | HTTP Method | URL              | Açıklama                 |
| --------------- | ----------- | ---------------- | ------------------------ |
| Kullanıcı kaydı | POST        | `/auth/register` | Yeni kullanıcı ekler     |
| Giriş           | POST        | `/auth/login`    | Token döner              |
| Randevu oluştur | POST        | `/appointments`  | Yeni randevu oluşturur   |
| Randevular      | GET         | `/appointments`  | Tüm randevuları listeler |

---

### 🔐 5️⃣ Kimlik Doğrulama (Auth)

* Kullanıcı giriş yaptığında JWT (token) verilir.
* Token her istekte gönderilir (header içinde).
* Backend token’ı doğrular, kullanıcı yetkili mi bakar.

---

### 🧰 6️⃣ Hata Yönetimi ve Middleware

Örnek: Token yoksa → 401 Unauthorized döndür.
Bunu **Guard** veya **Middleware** ile yaparsın.

---

### ☁️ 7️⃣ Yayına Alma (Deployment)

Proje bittikten sonra:

* Kodu GitHub’a atarsın.
* **Render / Railway / AWS** gibi bir servise yükler, online hale getirirsin.
* Frontend artık senin API’ne istek gönderebilir 🌐

---

## 🧱 6. Backend Yazarken Mantık

Backend yazarken **şu düşünce yapısı** ile hareket etmelisin:

1. “İstemci benden ne istiyor?” (istek)
2. “Bu isteği nasıl güvenli şekilde işlerim?” (kontrol)
3. “Bu işlem hangi veriyi etkiler?” (database)
4. “Sonuç olarak ne döndürmeliyim?” (cevap)

> Backend aslında **karar verme mantığıdır**.
> Frontend süslüdür, backend akıllıdır.

---

## 💡 7. Sıfırdan Öğrenmek İçin Önerilen Yol

1. **JavaScript / TypeScript temellerini** öğren
   (değişken, fonksiyon, if, loop, async-await)
2. **HTTP & REST mantığını** öğren
   (GET, POST, PUT, DELETE nedir?)
3. **NestJS** öğrenmeye başla
   (modül, controller, service kavramları)
4. **Veritabanı temellerini** öğren
   (tablolar, ilişkiler, id, foreign key)
5. **JWT, Auth, Middleware** gibi konulara geç
6. Son olarak: **gerçek bir mini proje** yap
   (örnek: Not Defteri API’si, Blog API’si, Randevu sistemi)

---

## 🎯 Özetle

Backend:

* “Veriyi yöneten ve frontend’e servis eden akıl katmanıdır.”
* Her şey **istek (request)** → **işlem (logic)** → **cevap (response)** döngüsüyle çalışır.
* Koddan önce **mantığı** anlamak her şeyden önemlidir.

---
