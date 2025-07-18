# Power BI Projesi  

## Özet
- Excel veri setinin Power BI'a aktarılması
- Veri temizleme ve modelleme işlemleri
- DAX ile yeni sütunların ve ölçülerin oluşturulması
- Verilerin grafikleştirilmesi

## Proje Dosyaları
| Dosya Adı                      | Açıklama |
|--------------------------------|----------|
| `Proje.pbix`                   | Power BI proje dosyam |
| `DAX Komutları.md`             | Verilerin analiz edilmesi için kullandığım komutlar |
| `PowerBI Projesi Sunumu.pdf`   | Projeye ait rapor çıktısı |
| `CSV Dosyaları`                | Projede kullanılan CSV dosyaları |
| `README.md`                    | Bu dosya 😎 |  
  

## Şehirler ve Bölgeler için kullandığım dosyaya buradan ulaşabilirsiniz:  
https://github.com/yigith/TurkiyeSehirlerBolgeler/blob/master/excel/SehirlerBolgeler.xlsx


## 🔧 Uygulanan Dönüşümler ve İşlemler
Projede 8 ana tabloya yönelik veri hazırlama adımları gerçekleştirilmiştir:
-**Users (Kullanıcılar) Tablosu**: Yaş hesaplama, cinsiyet sınıflandırması, ad-soyad ayrımı, gereksiz kolonların kaldırılması, gizleme işlemleri yapılmıştır.
- **Adres Tablosu**: USERID ve CITY alanları birleştirilmiş, kolon adlandırmaları güncellenmiştir.
- **Ürünler Tablosu**: Kategori isimlendirmeleri yenilenmiş, yeni bir üst kategori tanımı yapılmıştır.
- **Sipariş ve Sipariş Detay Tablosu**: Tarih formatları düzenlenmiş, veri türleri kontrol edilmiştir.
- **Bölgeler Tablosu**: Dış kaynaklı şehir-bölge eşleşmeleri modele dahil edilmiştir.

Modelleme adımlarında tablolar arasında ilişkilendirme ve kullanıcıya görünmemesi gereken alanların gizlenmesi sağlanmıştır.


## Seçilen Marka  
- PANTENE


## 📊 Oluşturulan Raporlar
Raporlama kısmı seçilen markaya göre 3 temel perspektife göre yapılmıştır:
- **📌 Özet Sayfa**: Satış adetleri, ciro, haftasonu-haftaiçi karşılaştırmaları, saatlik analizler vb.
- **👥 Müşteri Perspektifi**: Yaşa ve cinsiyete göre müşteri sayısı.
- **📦 Kategori Perspektifi**: Yaş gruplarına göre müşterilerin İstanbul'daki toplam cirosunun kategoriye göre dağılımı.




