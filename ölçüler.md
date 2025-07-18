# Power BI Projesinde Kullanılan DAX Komutları

Bu dosya, Power BI projesinde kullanılan ölçü ve sütun oluşturmak için kullanılan DAX komutlarını içermektedir.

---

## 🔢 1. Toplam Satış - ÖLÇÜ
```DAX
Toplam Satış = SUM('Sipariş_Detay'[AMOUNT])
```

## 🔢 2. Toplam Kazanç - ÖLÇÜ

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

## 🔢 3. Müşteri Sayısı - ÖLÇÜ 

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

## 🔢 4. Sipariş Sayısı - ÖLÇÜ
```DAX
Sipariş Sayısı = 
CALCULATE(
    DISTINCTCOUNT('Sipariş_Detay'[ORDERID]),
    FILTER('Sipariş_Detay', RELATED('Ürünler'[BRAND]) = "PANTENE")
)
```


## 🔢 5. Satılan Ürün Adeti - ÖLÇÜ
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


## 🔢 6. Müşteri Başına Satış Adeti - ÖLÇÜ
```DAX
Müşteri Başına Satış Adeti = DIVIDE([Satılan Ürün Adeti], [Müşteri Sayısı], 0)
```


## 🔢 7. Ortalama Sipariş Tutarı - ÖLÇÜ
```DAX
Ortalama Sipariş Tutarı = 
DIVIDE([Toplam Kazanç], [Sipariş Sayısı], 0)
```


## 🔢 8. Müşteri Başına Ciro - ÖLÇÜ
```DAX
Müşteri Başına Ciro = DIVIDE([Toplam Kazanç], [Müşteri Sayısı], 0)
```


## 🔢 9. Haftaiçi Toplam Satış - ÖLÇÜ
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

## 🔢 10. Haftasonu Toplam Satış - ÖLÇÜ
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


## 🔢 11. Pantene Kadın Müşteri Sayısı - ÖLÇÜ
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

## 🔢 12. Pantene Erkek Müşteri Sayısı - ÖLÇÜ
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

## 🔢 13. Yaş Grubu (Sütun oluşturma)
```DAX
Yaş Grubu = 
SWITCH(
    TRUE(),
    'Kullanıcılar'[YAS] <= 20, "Genç",
    'Kullanıcılar'[YAS] <= 35, "Yetişkin",
    'Kullanıcılar'[YAS] <= 55, "Orta Yaş",
    "Yaşlı"
)
```

## 🔢 14. Pantene İstanbul Genç Ciro - ÖLÇÜ
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

## 🔢 2. Toplam Kazanç  
```DAX

```


