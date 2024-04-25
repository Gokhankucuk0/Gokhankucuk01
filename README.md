SOLİD PRENSİPLERİ 
1-) Single Responsibility (Tek Sorumluluk Prensibi): Her sınıf veya modülün yalnızca bir görevi olmalıdır. Bu, kodun daha okunabilir, 
daha sürdürülebilir ve daha esnek olmasını sağlar.
-Uygulanmamış Hali

class KullaniciIslemleri:
    def bilgileri_kaydet(self, kullanici_adı, sifre, e_posta):
        # Kullanıcı bilgilerini veritabanına kaydet
        pass
    
    def oturum_ac(self, kullanici_adı, sifre):
        # Kullanıcıyı oturum aç
        pass

Yukarıdaki kodda, KullaniciIslemleri adlı sınıf hem kullanıcı bilgilerini kaydetme (bilgileri_kaydet metodu) hem de oturum açma
(oturum_ac metodu) işlemlerini gerçekleştiriyor. 
Bu durum Tek Sorumluluk Prensibi'ne aykırıdır
-Uygulanmış Hali

class KullaniciBilgileri:
    def kaydet(self, kullanici_adı, sifre, e_posta):
        # Kullanıcı bilgilerini veritabanına kaydet
        pass

class Oturum:
    def ac(self, kullanici_adı, sifre):
        # Kullanıcıyı oturum aç
        pass
Yukarıdaki kodda, KullaniciBilgileri sınıfı yalnızca kullanıcı bilgilerini kaydetme işlevini gerçekleştirirken, Oturum sınıfı 
yalnızca oturum açma işlevini gerçekleştiriyor.
Her sınıfın tek bir sorumluluğu vardır ve bu da Tek Sorumluluk Prensibi'ne uygun bir tasarım sağlar.

2-) Open/Closed Principle (Açık Kapalı Prensibi): Yazılım varlıkları (sınıflar, modüller, fonksiyonlar vb.), genişlemeye açık 
ancak değişikliğe kapalı olmalıdır. Yani mevcut kodu değiştirmeden yeni işlevsellik eklemek mümkün olmalıdır.

class Odeme:
    def yap(self, miktar):
        pass

class KrediKarti(Odeme):
    def yap(self, miktar):
        print(f"{miktar} TL kredi kartı ile ödeme yapıldı.")

class Havale(Odeme):
    def yap(self, miktar):
        print(f"{miktar} TL havale ile ödeme yapıldı.")

# Kullanımı görelim:
kredi_karti = KrediKarti()
havale = Havale()

kredi_karti.yap(100)  # 100 TL'lik alışveriş kredi kartı ile ödendi
havale.yap(150)       # 150 TL'lik alışveriş havale ile ödendi


Bu örnekte, Odeme adında bir soyut bir sınıf tanımladık ve bu sınıftan türetilen KrediKarti ve Havale sınıfları oluşturduk. 
Her iki sınıf da ödeme işlemini temsil eden yap yöntemini uygular.

KrediKarti ve Havale sınıflarını oluşturduktan sonra, her birinin yap yöntemini çağırarak ödeme işlemlerini gerçekleştirdik.
Bu sayede her bir ödeme yöntemi farklı bir şekilde işlendiğinden, Açık Kapalı Prensibi'ne uygun bir tasarım elde ettik.


3-) Liskov Substitution Principle (Liskov Yerine Koyma Prensibi): Bir üst sınıfın (üst tür) nesneleri, alt sınıfların 
(alt türler) nesneleriyle yer değiştirilebilir olmalıdır. Bu, kodun düzgün çalışmasını ve beklenmedik davranışlardan kaçınmayı sağlar.

class Sekil:
    def alan_hesapla(self):
        pass

class Dikdortgen(Sekil):
    def __init__(self, uzunluk, genislik):
        self.uzunluk = uzunluk
        self.genislik = genislik
    
    def alan_hesapla(self):
        return self.uzunluk * self.genislik

class Kare(Sekil):
    def __init__(self, kenar_uzunlugu):
        self.kenar_uzunlugu = kenar_uzunlugu
    
    def alan_hesapla(self):
        return self.kenar_uzunlugu ** 2

# Kullanımı görelim:
dikdortgen = Dikdortgen(5, 4)
kare = Kare(3)

print("Dikdörtgenin Alanı:", dikdortgen.alan_hesapla()) 
print("Karenin Alanı:", kare.alan_hesapla()) 

Bu örnekte, "Sekil" sınıfı üst sınıfı temsil ederken, "Dikdortgen" ve "Kare" sınıfları bu üst sınıfın yerine geçebilir. 
Her iki alt sınıf da "alan_hesapla" metodunu uygulayarak üst sınıfın davranışını korur. 
Bu, Liskov Yerine Koyma Prensibi'ne uygun bir tasarımdır.

4-) Interface Segregation Principle (Arayüz Ayırma Prensibi): Bir sınıfın, ihtiyaç duymadığı yöntemleri içermemesi
gerekmektedir. Bu, sınıflar arasındaki sıkı bağımlılığı azaltır ve daha esnek kod yazmayı sağlar.

Arayüz Ayırma Prensibi, bir sınıfın, ihtiyaç duymadığı yöntemleri içermemesi gerektiğini belirtir. Bu prensip, bir
sınıfın sadece kullandığı yöntemlere odaklanmasını ve gereksiz karmaşıklığı önlemesini sağlar.

-Uygulanmış Hali

from abc import ABC, abstractmethod

class Araba(ABC):
    def __init__(self, marka):
        self.marka = marka

    @abstractmethod
    def hareket_et(self):
        pass

class Tesla(Araba):
    def hareket_et(self):
        print(f"{self.marka} hareket ediyor: Elektrikli motor çalıştı.")

class BMW(Araba):
    def hareket_et(self):
        print(f"{self.marka} hareket ediyor: Benzinli motor çalıştı.")

arabalar = [Tesla("Tesla Model S"), BMW("BMW 3 Serisi")]

for araba in arabalar:
    araba.hareket_et()
    
Bu düzenlemeyle, alt sınıflar (Tesla ve BMW) üst sınıfın (Araba) hareket etme metodu olan hareket_et metodunu uygularlar.
Bu şekilde, Liskov Yerine Koyma Prensibi'ne uygun bir tasarım elde edilir.

-Uygulanmamış Hali

class Araba:
    def __init__(self, marka):
        self.marka = marka

    def motoru_calistir(self):
        pass

    def hareket_et(self):
        pass

class Tesla(Araba):
    def motoru_calistir(self):
        print(f"{self.marka} motoru çalıştı: Elektrikli motor çalıştı.")

    def hareket_et(self):
        print(f"{self.marka} hareket ediyor: Elektrikli motor çalıştı.")

class BMW(Araba):
    def motoru_calistir(self):
        print(f"{self.marka} motoru çalıştı: Benzinli motor çalıştı.")

    def hareket_et(self):
        print(f"{self.marka} hareket ediyor: Benzinli motor çalıştı.")

arabalar = [Tesla("Tesla Model S"), BMW("BMW 3 Serisi")]

for araba in arabalar:
    araba.motoru_calistir()
    araba.hareket_et()

Bu örnekte, alt sınıflar (Tesla ve BMW) üst sınıfın (Araba) metotlarını ezerek kendi özelliklerini uyguluyorlar. 
Bu durumda, alt sınıflar üst sınıfların yerine kullanıldığında beklenmeyen davranışlar ortaya çıkabilir, bu da 
Liskov Yerine Koyma Prensibi'ne aykırıdır.

-Uygulanmış Hali

class Araba:
    def __init__(self, marka):
        self.marka = marka

    def motoru_calistir(self):
        print("Motor çalıştı.")

    def hareket_et(self):
        print("Araba hareket ediyor.")

class Tesla(Araba):
    def __init__(self, marka):
        super().__init__(marka)

    def motoru_calistir(self):
        print(f"{self.marka} motoru çalıştı: Elektrikli motor çalıştı.")

    def hareket_et(self):
        print(f"{self.marka} hareket etti: Elektrikli motorla.")

class BMW(Araba):
    def __init__(self, marka):
        super().__init__(marka)

    def motoru_calistir(self):
        print(f"{self.marka} motoru çalıştı: Benzinli motor çalıştı.")

    def hareket_et(self):
        print(f"{self.marka} hareket etti: Benzinli motorla.")

for araba in [Tesla("Tesla Model S"), BMW("BMW 3 Serisi")]:
    araba.motoru_calistir()
    araba.hareket_et()

    Bu kodda, Tesla ve BMW sınıfları, Araba sınıfından türetilmiş ve kendi özelliklerini eklemişlerdir.
    Her iki alt sınıf da üst sınıfın metotlarını kullanmış, böylece Liskov Yerine Koyma Prensibi'ne uygun bir tasarım elde edilmiştir.

5-) Dependency Inversion Principle (Bağımlılıkları Ters Çevirme Prensibi): Yüksek seviyeli modüller, 
düşük seviyeli modüllere bağımlı olmamalıdır. Her iki seviye de soyutlamalara (interfacelere) bağımlı olmalıdır. 
Bu, kodun daha modüler, esnek ve yeniden kullanılabilir olmasını sağlar.

Bağımlılıkları Ters Çevirme Prensibi, üst seviye modüllerin alt seviye modüllere bağlı olmamasını,
her ikisinin de soyutlamalara (interface'lere) bağlı olması gerektiğini söyler. 

-Uygulanmış Hali

class Eyleyici:
    def calistir(self):
        pass

class Kapi:
    def __init__(self, eyleyici):
        self.eyleyici = eyleyici

    def ac(self):
        self.eyleyici.calistir()

class Lamba:
    def __init__(self, eyleyici):
        self.eyleyici = eyleyici

    def yak(self):
        self.eyleyici.calistir()

class Anahtar:
    def calistir(self):
        print("Anahtar çalıştırıldı: Lamba yandı.")

eyleyici = Anahtar()
kapi = Kapi(eyleyici)
lamba = Lamba(eyleyici)

kapi.ac()   # Kapiyi acar, eylem Anahtar'a gider, Lambayi yakar
lamba.yak() # Lambayi yakar, eylem yine Anahtar'a gider

-Uygulanmamış Hali

class Kapi:
    def ac(self):
        print("Kapı açıldı.")

class Lamba:
    def yak(self):
        print("Lamba yandı.")

class Anahtar:
    def kontrol_et(self):
        print("Anahtar kontrol edildi.")

    def ac_kapı(self, kapi):
        print("Anahtar çalıştırıldı: Kapı açıldı.")
        kapi.ac()

    def yak_lamba(self, lamba):
        print("Anahtar çalıştırıldı: Lamba yandı.")
        lamba.yak()

anahtar = Anahtar()
kapi = Kapi()
lamba = Lamba()

anahtar.ac_kapı(kapi)
anahtar.yak_lamba(lamba)


Bu örnekte, Anahtar sınıfı, Kapi ve Lamba sınıflarına doğrudan bağlıdır. Yani, Anahtar sınıfı Kapi ve Lamba 
sınıflarını doğrudan çağırır ve onları kontrol eder. Bu durumda, üst seviye modül olan Anahtar alt seviye 
modüllere (Kapi ve Lamba) bağımlı hale gelir ve Bağımlılıkları Ters Çevirme Prensibi'ne uymaz.
