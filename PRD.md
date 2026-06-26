# Ürün Gereksinimleri Belgesi (PRD) - Yatırım Finansman Mobile Release Reporter

## 1. Uygulama Ne İşe Yarar? (Ürün Özeti)
**Yatırım Finansman Mobile Release Reporter**, Jira'dan dışa aktarılan (HTML veya Excel formatlı) sürüm listelerini anında okunabilir, kurumsal şablona uygun ve Outlook/Mac Mail ile tam uyumlu **Sürüm Notları (Release Notes)** formatına dönüştüren yerel bir web uygulamasıdır.

Bu araç; raporları manuel oluşturma, kopyala-yapıştır yapma, bilet tipi ve platform tespiti gibi zaman alan el yordamı süreçleri otomatikleştirerek raporlama doğruluğunu artırır.

---

## 2. Sistem Nasıl Çalışır ve Verileri Nasıl Sınıflandırır?

Uygulamanın veri ayrıştırma, gruplama ve sınıflandırma kuralları aşağıda detaylandırılmıştır:

### A. Talepler (Stories & Change Requests)
- **Kapsam**: Yalnızca Jira kayıt türü `Story` veya `Change Request` olan biletler bu bölüme dahil edilir.
- **İlişkili Backlog ID**:
  - Satır/bağlantı hücrelerinde `YFMB-XXXX` formatında bir bilet anahtarı aranır.
  - Eğer `YFMB` bağlı bilet anahtarı bulunursa, sol sütundaki **Backlog ID** hücresine yazılır.
  - Eğer biletin kendisinde veya bağlı olduğu biletler arasında `YFMB` anahtarı yoksa, bu sütunda `-` gösterilmeye devam edilir.
- **Gruplama**: Talepler, ait oldukları "Epic Adı" (Epic Name / Destan) değerine göre gruplanarak gösterilir.

### B. Tamamlanan Kayıtlar (UAT Bugs & New Requests & Improvements)
- **Kapsam**: Yalnızca Jira kayıt türü `Bug` veya `Improvement` olan biletler bu bölüme dahil edilir.
- **Filtreleme Kuralı**: Bu kayıtlar arasından yalnızca **Linked Issues alanında bir `YFMB` bilet anahtarı bağlı olanlar** (Backlog ID hücresi boş / `-` olmayanlar) rapora yansıtılır. YFMB bağı olmayan hata veya geliştirmeler müşterinin takip edebilmesi için elenir.
- **Kolon Başlığı**: Sol kolonun adı **Backlog ID** (eski adıyla Defect ID) olarak adlandırılır ve burada ilişkili `YFMB-XXXX` bilet anahtarı yer alır.
- **Açıklama (Summary)**: Bu kolonda, Jira'dan gelen asıl hata biletinin (örneğin `YFMFLR-XXXX` veya `YFMMW-XXXX` biletlerinin) kendi özet/başlık bilgisi (summary) gösterilir.

### C. Geliştirme Platformu Algılama
Sistem, yüklenen dosyadaki bilet anahtarlarını (Issue Key) analiz ederek platformu tahmin eder:
- Bilet anahtarında `ANDROID` veya `AND` geçiyorsa: `ANDROID`
- Bilet anahtarında `IPHONE` veya `IOS` geçiyorsa: `IOS`
- Bilet anahtarında `FLUTTER` veya `FLT` geçiyorsa: `FLUTTER`
- Bilet anahtarında `MW` veya `MID` geçiyorsa ya da eşleşme yoksa varsayılan olarak `Mobile + BFF + MW` yansıtılır.
- *Not: Tespit edilen platform değeri, arayüzdeki "Platform" alanından kullanıcı tarafından elle değiştirilebilir.*

### D. Paket Detayları (Kısım C)
- **Yapı**: Kısım C, **YAFI Preprod Paket Detayları** ve **YAFI UAT Paket Detayları** olmak üzere iki tablo halinde yapılandırılmıştır. Her bir tablo içinde **iOS Paket Bilgileri** ve **Android Paket Bilgileri** başlıkları altında iki sütun yan yana (50% - 50% genişlikte) yer alır.
- **Sürüm / Build Parametreleri**: Şablondaki tüm sürüm numaraları (örneğin `1.4.0.17` veya `1.4.0.16` yerine) mavi ve italik formatta **`{Fix_Build}`** placeholder ifadesiyle listelenir. Bu alanlar kopyalandıktan sonra veya ekran üzerinden el ile güncellenebilir.
- **iOS Adımları**: Kurulum adımları numaralı liste (`<ol>`) yerine, bullet (nokta - `<ul>`) listesi şeklinde gösterilir.
- **Android Paket Durumu**: Android indirme kutusu (Pozitron tablosu) tek bir yeşil/turkuaz renkli hücreden oluşur ve içinde kalın ve italik olarak **"Android paketini eklemeyi unutma!!!"** metni yer alır.
- **İndirme Linkleri**: URL kısmında parantez içindeki ek yönergeler kaldırılarak doğrudan temiz bir şekilde `URL: https://dist.pozitron.com` bağlantısı sunulur. Kullanıcı adı olarak `yatfin` bilgisi yer alır.

### E. Jira Filtresi (JQL)
- **Açıklama**: Arayüzdeki **Jira Filter** butonu, kullanıcıyı aşağıdaki JQL sorgusuyla filtrelenmiş Jira sayfasına yönlendirir:
  ```sql
  project IN ("Yatırım Finansman Mobile MW", "Yatırım Finansman Mobile Flutter")
  AND issuetype IN (Story, "Change Request", Improvement,Bug)
  AND fixVersion = "1.4.0"
  AND (
      status = Approved
      OR (
          status = Closed
          AND resolution IN (Fixed, Done)
      )
  )
  ORDER BY statusCategoryChangedDate DESC
  ```

---

## 3. Ekran Özellikleri ve Kullanıcı Düzenlemeleri

Rapor oluşturulduktan sonra ekran üzerinde tüm alanlara müdahale edilebilir. Yapılan düzenlemeler hem PDF'e hem de kopyalanan mail HTML'ine anında yansır:
- **Proje Bilgisi Düzenleme**: Varsayılan olarak "Yatırım Finansman Mobile" gelir ancak üzerine tıklanarak "Yatırım Finansman Mobile MW" veya "Yatırım Finansman Mobile Flutter" gibi değiştirilebilir.
- **Sürüm Bilgisi Düzenleme**: Jira'dan okunan sürüm bilgisi elle düzenlenebilir.
- **Platform Bilgisi Düzenleme**: Otomatik algılanan platform değeri değiştirilebilir.
- **Tarih Bazlı Filtreleme**: Listelenen biletlerin yanındaki tarihlere tıklanarak, o tarihten önce tamamlanmış biletler pasif (gri ve eğik yazı) hale getirilebilir. Bu sayede müşteriye daha önce iletilmiş olan biletler görsel olarak ayırt edilir.
- **Hata Açıklamaları**: Tablolardaki hata açıklaması hücreleri tıklandığında otomatik boyutlanan metin kutularına dönüşür ve elle düzenlenebilir.
- **Sürüm Detayları (Belirtilmesi Gerekenler / Bilinen Durumlar)**: Jira'dan otomatik çekilen sürüm notları veya bilinen durum bilgileri doğrudan ekrandan düzenlenebilir/eklenebilir.
- **Paket Detayları Düzenleme (Kısım C)**: Preprod ve UAT şablonları üzerinde yer alan {Fix_Build} değerleri, Pozitron linkleri, kullanıcı bilgileri ve davet adımları doğrudan ekran üzerinden tıklanarak düzenlenebilir.

---

## 4. Raporu Dışa Aktarma (Export)

- **Mail İçin Kopyala**: Outlook ve Mac Mail gibi masaüstü istemcilerle %100 uyumlu zengin HTML kodunu panoya kopyalar. Outlook'a yapıştırıldığında tablonun genişliği (`794px`), hücre kenarlıkları, renkleri ve link yapısı mükemmel şekilde korunur.
- **PDF İndir**: Rapor sayfasını A4 boyutlarında resmi bir PDF dokümanı olarak yerel bilgisayara kaydeder.

---

## 5. Proje Ortamı & Dağıtım Kuralları

- **Lokal Çalıştırma**: `/Users/puraligilm/Documents/Reporter` dizininde terminalden `npm run dev` komutu verilerek yerel sunucu başlatılır.
- **Derleme Kontrolü**: Kodlarda hata olmadığını doğrulamak için `npm run build` komutu kullanılabilir.
- **Dağıtım (Deployment)**: Proje GitHub Pages üzerinden `https://yafiqa.github.io/reporter/` adresinde barındırılmaktadır. Reponun `main` veya `master` dallarına yapılan her `push` işleminde, GitHub Actions (`.github/workflows/deploy.yml`) projeyi otomatik olarak derler ve yayına alır.
