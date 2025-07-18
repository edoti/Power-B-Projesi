# Power BI Projesinde Kullanılan DAX Komutları

Bu dosya, Power BI projesinde kullanılan ölçü ve sütun oluşturmak için kullanılan DAX komutlarını içermektedir.

---
## Sütun oluşturmak için kullanılan DAX Komutları

### 1. AD Sütunu 
```DAX
AD = 
VAR TamAd = 'Kullanıcılar'[NAMESURNAME]
VAR BoslukSayisi = LEN(TamAd) - LEN(SUBSTITUTE(TamAd, " ", ""))
VAR SonBoslukYeri = FIND("¤", SUBSTITUTE(TamAd, " ", "¤", BoslukSayisi), 1)
RETURN 
    IF(
        BoslukSayisi = 0,
        BLANK(),
        LEFT(TamAd, SonBoslukYeri - 1)
    )
```

### 2. SOYAD Sütunu 
```DAX
SOYAD = 
VAR TamAd = 'Kullanıcılar'[NAMESURNAME]
VAR BoslukSayisi = LEN(TamAd) - LEN(SUBSTITUTE(TamAd, " ", ""))
VAR SonBoslukYeri = FIND("¤", SUBSTITUTE(TamAd, " ", "¤", BoslukSayisi), 1)
RETURN 
    IF(
        BoslukSayisi = 0,
        TamAd,
        MID(TamAd, SonBoslukYeri + 1, LEN(TamAd))
    )
```

### 3. CİNSİYET Sütunu 
```DAX
CİNSİYET = SWITCH(TRUE(), 'Kullanıcılar'[GENDER] = "E", "ERKEK", 'Kullanıcılar'[GENDER] = "K" ,"KADIN")
```

### 4. KULLANICI_ŞEHİR Sütunu 
```DAX
KULLANICI_ŞEHİR = FORMAT(Adres[USERID],"0") & "-" & Adres[ŞEHİR]
```

### 5. YAS Sütunu 
```DAX
YAS = DATEDIFF('Kullanıcılar'[BIRTHDATE], TODAY(),YEAR)
```

### 6. YAS_GRUBU (Sütun oluşturma)
```DAX
YAS_GRUBU = 
SWITCH(
    TRUE(),
    'Kullanıcılar'[YAS] <= 20, "Genç",
    'Kullanıcılar'[YAS] <= 35, "Yetişkin",
    'Kullanıcılar'[YAS] <= 55, "Orta Yaş",
    "Yaşlı"
)
```

### 7. PASSWORD Sütunu 
```DAX
PASSWORD = UPPER('Kullanıcılar'[PASSWORD_])
```

### 8. BOLGEAD_  Sütunu 
```DAX
BOLGEAD_ = UPPER('Bölgeler'[BolgeAd])
```

### 9. HİÇİ_HSONU  Sütunu 
```DAX
HİÇİ_HSONU = 
VAR Gun = WEEKDAY('Sipariş'[DATE], 2) -- Pazartesi=1, Pazar=7
RETURN IF(Gun <= 5, "Haftaiçi", "Haftasonu")
```

### 10. SAAT Sütunu 
```DAX
SAAT = HOUR('Sipariş'[DATE_])
```

### 11. SEHİR_AD Sütunu 
```DAX
SEHİR_AD = UPPER('Şehirler'[SehirAd])
```

### 12. YENIANAKATEGORI Sütunu 
```DAX
YENIANAKATEGORI = 
SWITCH(
    TRUE(),
    'Ürünler'[ANAKATEGORI] = "SOGUK ICECEKLER", "ICECEKLER",
    'Ürünler'[ANAKATEGORI] = "CAY-KAHVE-SEKER", "ICECEKLER",
    'Ürünler'[ANAKATEGORI] = "SICAK ICECEKLER", "ICECEKLER",
    'Ürünler'[ANAKATEGORI]  // Diğerleri orijinal hâliyle
)

```




---
---


## Ölçü oluşturmak için kullanılan DAX Komutları

### 🔢 1. Toplam Satış - ÖLÇÜ
```DAX
Toplam Satış = SUM('Sipariş_Detay'[AMOUNT])
```

### 🔢 2. Toplam Kazanç - ÖLÇÜ
```DAX
Toplam Kazanç = 
CALCULATE(
    SUMX(
        'Sipariş_Detay',
        'Sipariş_Detay'[AMOUNT] * RELATED('Ürünler'[UNITPRICE])
    ),
    FILTER(
        'Sipariş_Detay',
        RELATED('Ürünler'[BRAND]) = "PANTENE"
    )
)
```

### 🔢 3. Müşteri Sayısı - ÖLÇÜ 
```DAX
Müşteri Sayısı = 
CALCULATE(
    DISTINCTCOUNT('Sipariş'[USERID]),
    FILTER(
        'Sipariş_Detay',
        RELATED('Ürünler'[BRAND]) = "PANTENE"
    )
)
```

### 🔢 4. Sipariş Sayısı - ÖLÇÜ
```DAX
Sipariş Sayısı = 
CALCULATE(
    DISTINCTCOUNT('Sipariş_Detay'[ORDERID]),
    FILTER('Sipariş_Detay', RELATED('Ürünler'[BRAND]) = "PANTENE")
)
```

### 🔢 5. Satılan Ürün Adeti - ÖLÇÜ
```DAX
Satılan Ürün Adeti = 
CALCULATE(
    SUM('Sipariş_Detay'[AMOUNT]),
    FILTER(
        'Sipariş_Detay',
        RELATED('Ürünler'[BRAND]) = "PANTENE"
    )
)
```

### 🔢 6. Müşteri Başına Satış Adeti - ÖLÇÜ
```DAX
Müşteri Başına Satış Adeti = DIVIDE([Satılan Ürün Adeti], [Müşteri Sayısı], 0)
```

### 🔢 7. Ortalama Sipariş Tutarı - ÖLÇÜ
```DAX
Ortalama Sipariş Tutarı = 
DIVIDE([Toplam Kazanç], [Sipariş Sayısı], 0)
```


### 🔢 8. Müşteri Başına Ciro - ÖLÇÜ
```DAX
Müşteri Başına Ciro = DIVIDE([Toplam Kazanç], [Müşteri Sayısı], 0)
```


### 🔢 9. Haftaiçi Toplam Satış - ÖLÇÜ
```DAX
Haftaiçi Toplam Satış = 
CALCULATE(
    SUMX(
        'Sipariş_Detay',
        'Sipariş_Detay'[AMOUNT] * RELATED('Ürünler'[UNITPRICE])
    ),
    FILTER(
        'Sipariş_Detay',
        RELATED('Ürünler'[BRAND]) = "PANTENE"
    ),
    TREATAS({"Haftaiçi"}, 'Sipariş'[HİÇİ_HSONU])
)
```

### 🔢 10. Haftasonu Toplam Satış - ÖLÇÜ
```DAX
Haftasonu Toplam Satış = 
CALCULATE(
    SUMX(
        'Sipariş_Detay',
        'Sipariş_Detay'[AMOUNT] * RELATED('Ürünler'[UNITPRICE])
    ),
    FILTER(
        'Sipariş_Detay',
        RELATED('Ürünler'[BRAND]) = "PANTENE"
    ),
    TREATAS({"Haftasonu"}, 'Sipariş'[HİÇİ_HSONU])
)
```


### 🔢 11. Pantene Kadın Müşteri Sayısı - ÖLÇÜ
```DAX
Pantene Kadın Müşteri Sayısı = 
CALCULATE(
    DISTINCTCOUNT('Sipariş'[USERID]),
    FILTER(
        'Sipariş_Detay',
        RELATED('Ürünler'[BRAND]) = "PANTENE"
    ),
    TREATAS({"KADIN"}, 'Kullanıcılar'[CİNSİYET])
)
```

### 🔢 12. Pantene Erkek Müşteri Sayısı - ÖLÇÜ
```DAX
Pantene Erkek Müşteri Sayısı = 
CALCULATE(
    DISTINCTCOUNT('Sipariş'[USERID]),
    FILTER(
        'Sipariş_Detay',
        RELATED('Ürünler'[BRAND]) = "PANTENE"
    ),
    TREATAS({"ERKEK"}, 'Kullanıcılar'[CİNSİYET])
)
```



### 🔢 13. Pantene İstanbul Genç Ciro - ÖLÇÜ
```DAX
Pantene İstanbul Genç Ciro = 
CALCULATE(
    SUMX(
        'Sipariş_Detay',
        'Sipariş_Detay'[AMOUNT] * RELATED('Ürünler'[UNITPRICE])
    ),
    FILTER(
        'Ürünler',
        'Ürünler'[BRAND] = "PANTENE"
    ),
    FILTER(
        'Kullanıcılar',
        'Kullanıcılar'[Yaş Grubu] = "Genç"
    ),
    FILTER(
        'Adres',
        'Adres'[ŞEHİR] = "İSTANBUL"
    )
)
```


---


