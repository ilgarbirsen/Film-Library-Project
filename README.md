import sqlite3

class Kullanici:
    def __init__(self, ad, soyad, email, kullanici_adi, sifre):
        self.ad = ad
        self.soyad = soyad
        self.email = email
        self.kullanici_adi = kullanici_adi
        self.sifre = sifre

    def bilgileri_goster(self):
        print("\n Kullanıcı Bilgileri: ")
        print(f"""
            Ad: {self.ad}
            Soyad: {self.soyad}
            Email: {self.email}
            Kullanıcı Adı: {self.kullanici_adi}
            Şifre: ********
        """)

    def bilgileri_guncelle(self):
        yeni_ad = input("Kullanıcının Yeni adını giriniz (boş bırakırsanız eski ad kaydedilmiş olacaktır): ")
        yeni_soyad = input("Kullanıcının Yeni soyadını giriniz (boş bırakırsanız eski soyad kaydedilmiş olacaktır): ")
        yeni_email = input("Kullanıcının Yeni emailini giriniz (boş bırakırsanız eski email kaydedilmiş olacaktır): ")
        yeni_kullanici_adi = input("Kullanıcının Yeni kullanıcı adını giriniz (boş bırakırsanız eski kullanıcı ad kaydedilmiş olacaktır): ")
        yeni_sifre = input("Kullanıcının Yeni şifresini giriniz (boş bırakırsanız eski şifre kaydedilmiş olacaktır): ")

        if yeni_ad:
            self.ad = yeni_ad
        if yeni_soyad:
            self.soyad = yeni_soyad
        if yeni_email:
            self.email = yeni_email
        if yeni_kullanici_adi:
            self.kullanici_adi = yeni_kullanici_adi
        if yeni_sifre:
            self.sifre = yeni_sifre
        print("Kullanıcı bilgileri başarılı bir şekilde güncellendi.")


class Yonetmen:
    def __init__(self, yonetmen_ad_soyad, dogum_yili):
        self.yonetmen_ad_soyad = yonetmen_ad_soyad
        self.dogum_yili = dogum_yili

    def bilgileri_goster(self):
        print("\nYönetmen Bilgileri: ")
        print(f"""
            Ad ve Soyad: {self.yonetmen_ad_soyad}
            Doğum Yılı: {self.dogum_yili}
        """)

    def bilgileri_guncelle(self):
        yeni_ad_soyad = input("Yönetmenin Yeni adı ve soyadını giriniz (boş bırakırsanız eski ad ve soyad kaydedilmiş olacaktır): ")
        yeni_dogum_yili = input("Yönetmenin Yeni doğum yılını giriniz (boş bırakırsanız eski doğum yılı kaydedilmiş olacaktır): ")

        if yeni_ad_soyad:
            self.yonetmen_ad_soyad = yeni_ad_soyad
        if yeni_dogum_yili:
            self.dogum_yili = yeni_dogum_yili
        print("Yönetmen bilgileri başarılı bir şekilde güncellendi.")


class Film:
    def __init__(self, film_adi, yonetmen_ad_soyad, cikis_yili, tur):
        self.film_adi = film_adi
        self.yonetmen_ad_soyad = yonetmen_ad_soyad
        self.cikis_yili = cikis_yili
        self.tur = tur

    def bilgileri_goster(self):
        print("\nFilm Bilgileri: ")
        print(f"""
            Film Adı: {self.film_adi}
            Yönetmen Adı ve Soyadı: {self.yonetmen_ad_soyad}
            Çıkış Yılı: {self.cikis_yili}
            Tür: {self.tur}
        """)

    def bilgileri_guncelle(self):
        yeni_film_adi = input("Filmin Yeni adını giriniz (boş bırakırsanız eski ad kaydedilmiş olacaktır): ")
        yeni_yonetmen_ad_soyad = input("Filmin Yeni yönetmeninin ad ve soyadını giriniz (boş bırakırsanız eski yönetmen kaydedilmiş olacaktır): ")
        yeni_cikis_yili = input("Filmin Yeni çıkış yılını giriniz (boş bırakırsanız eski çıkış yılı kaydedilmiş olacaktır): ")
        yeni_tur = input("Filmin Yeni türünü giriniz (boş bırakırsanız eski türü kaydedilmiş olacaktır): ")

        if yeni_film_adi:
            self.film_adi = yeni_film_adi
        if yeni_yonetmen_ad_soyad:
            yonetmen = film_kutuphanesi.yonetmen_aramak(yeni_yonetmen_ad_soyad)
            if yonetmen:
                self.yonetmen_ad_soyad = yeni_yonetmen_ad_soyad
            else:
                print("Girmiş olduğunuz yönetmen bilgileri bulunamadı. Lütfen öncelikle yönetmeni sisteme ekleyin daha sonrasında film güncellemesi yapınız.")
        if yeni_cikis_yili:
            self.cikis_yili = yeni_cikis_yili
        if yeni_tur:
            self.tur = yeni_tur
        print("Film bilgileri başarılı bir şekilde güncellendi.")


class FilmKutuphanesi:
    def __init__(self):
        self.connect = sqlite3.connect('film_kutuphanesi.db')
        self.cursor = self.connect.cursor()
        self.create_tables()

    def create_tables(self):
        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS Kullanici (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                ad VARCHAR(100),
                soyad VARCHAR(100),
                email VARCHAR(100),
                kullanici_adi VARCHAR(100),
                sifre VARCHAR(50)
            )
        ''')

        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS Yonetmen (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                ad_soyad VARCHAR(200),
                dogum_yili INTEGER
            )
        ''')

        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS Film (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                film_adi TEXT,
                yonetmen_ad_soyad VARCHAR(200),
                cikis_yili INTEGER,
                tur VARCHAR(100),
                FOREIGN KEY (yonetmen_ad_soyad) REFERENCES Yonetmen(ad_soyad)
            )
        ''')

        self.connect.commit()

    def kullanici_ekle(self, kullanici):
        self.cursor.execute('''
            INSERT INTO Kullanici (ad, soyad, email, kullanici_adi, sifre)
            VALUES (?, ?, ?, ?, ?)
        ''', (kullanici.ad, kullanici.soyad, kullanici.email, kullanici.kullanici_adi, kullanici.sifre))
        self.connect.commit()
        print(f" {kullanici.kullanici_adi} başarılı bir şekilde eklendi.")

    def yonetmen_ekle(self, yonetmen):
        self.cursor.execute('''
            INSERT INTO Yonetmen (ad_soyad, dogum_yili)
            VALUES (?, ?)
        ''', (yonetmen.yonetmen_ad_soyad, yonetmen.dogum_yili))
        self.connect.commit()
        print(f"{yonetmen.yonetmen_ad_soyad} başarılı bir şekilde eklendi.")

    def film_ekle(self, film):
        self.cursor.execute('''
            INSERT INTO Film (film_adi, yonetmen_ad_soyad, cikis_yili, tur)
            VALUES (?, ?, ?, ?)
        ''', (film.film_adi, film.yonetmen_ad_soyad, film.cikis_yili, film.tur))
        self.connect.commit()
        print(f"{film.film_adi} başarılı bir şekilde eklendi.")

    def kullanicilari_goster(self):
        rows = self.cursor.execute('SELECT * FROM Kullanici').fetchall()
        for row in rows:
            kullanici = Kullanici(row[1], row[2], row[3], row[4], row[5])
            kullanici.bilgileri_goster()

    def yonetmenleri_goster(self):
        rows = self.cursor.execute('SELECT * FROM Yonetmen').fetchall()
        for row in rows:
            yonetmen = Yonetmen(row[1], row[2])
            yonetmen.bilgileri_goster()

    def filmleri_goster(self):
        rows = self.cursor.execute('SELECT * FROM Film').fetchall()
        for row in rows:
            film = Film(row[1], row[2], row[3], row[4])
            film.bilgileri_goster()

    def kullanici_sil(self, kullanici_adi):
        self.cursor.execute('DELETE FROM Kullanici WHERE kullanici_adi = ?', (kullanici_adi,))
        self.connect.commit()
        print(f"{kullanici_adi} başarılı bir şekilde silindi.")

    def yonetmen_sil(self, yonetmen_ad_soyad):
        self.cursor.execute('DELETE FROM Yonetmen WHERE ad_soyad = ?', (yonetmen_ad_soyad,))
        self.connect.commit()
        print(f"{yonetmen_ad_soyad} başarılı bir şekilde silindi.")

    def film_sil(self, film_adi):
        self.cursor.execute('DELETE FROM Film WHERE film_adi = ?', (film_adi,))
        self.connect.commit()
        print(f"{film_adi} başarılı bir şekilde silindi.")

    def film_aramak(self, film_adi):
        row = self.cursor.execute('SELECT * FROM Film WHERE film_adi = ?', (film_adi,)).fetchone()
        if row:
            return Film(row[1], row[2], row[3], row[4])
        return None
    
    def yonetmen_aramak(self, yonetmen_ad_soyad):
        row = self.cursor.execute('SELECT * FROM Yonetmen WHERE ad_soyad = ?', (yonetmen_ad_soyad,)).fetchone()
        if row:
            return Yonetmen(row[1], row[2])
        return None

    def kullanici_guncelleme(self, kullanici_adi):
        row = self.cursor.execute('SELECT * FROM Kullanici WHERE kullanici_adi = ?', (kullanici_adi,)).fetchone()
        if row:
            kullanici = Kullanici(row[1], row[2], row[3], row[4], row[5])
            kullanici.bilgileri_guncelle()
            self.cursor.execute('''
                UPDATE Kullanici
                SET ad = ?, soyad = ?, email = ?, kullanici_adi = ?, sifre = ?
                WHERE kullanici_adi = ?
            ''', (kullanici.ad, kullanici.soyad, kullanici.email, kullanici.kullanici_adi, kullanici.sifre, kullanici_adi))
            self.connect.commit()
        else:
            print("Kullanıcı bulunamadı.")

    def yonetmen_guncelleme(self, yonetmen_ad_soyad):
        row = self.cursor.execute('SELECT * FROM Yonetmen WHERE ad_soyad = ?', (yonetmen_ad_soyad,)).fetchone()
        if row:
            yonetmen = Yonetmen(row[1], row[2])
            yonetmen.bilgileri_guncelle()
            self.cursor.execute('''
                UPDATE Yonetmen
                SET ad_soyad = ?, dogum_yili = ?
                WHERE ad_soyad = ?
            ''', (yonetmen.yonetmen_ad_soyad, yonetmen.dogum_yili, yonetmen_ad_soyad))
            self.connect.commit()
        else:
            print("Yönetmen bulunamadı.")

    def film_guncelleme(self, film_adi):
        row = self.cursor.execute('SELECT * FROM Film WHERE film_adi = ?', (film_adi,)).fetchone()
        if row:
            film = Film(row[1], row[2], row[3], row[4])
            film.bilgileri_guncelle()
            self.cursor.execute('''
                UPDATE Film
                SET film_adi = ?, yonetmen_ad_soyad = ?, cikis_yili = ?, tur = ?
                WHERE film_adi = ?
            ''', (film.film_adi, film.yonetmen_ad_soyad, film.cikis_yili, film.tur, film_adi))
            self.connect.commit()
        else:
            print("Film bulunamadı.")

    def close_connection(self):
        self.connect.close()

film_kutuphanesi = FilmKutuphanesi()

while True:
    print(20 * "*", "Film Kütüphanesi Yönetim Sistemi", 20 * "*")
    print("""
        Lütfen yapacağınız işlemin numarasını giriniz:
        
        1. Kullanıcı Ekle
        2. Yönetmen Ekle
        3. Film Ekle
        4. Kullanıcı Listesi
        5. Yönetmen Listesi
        6. Film Listesi
        7. Kullanıcı Sil
        8. Yönetmen Sil
        9. Film Sil
        10. Kullanıcı Güncelleme
        11. Yönetmen Güncelleme
        12. Film Güncelleme
        13. Film Aramak (Filmin Adı ile)
        14. Çıkış (q)
    """)

    islem = input("Lütfen yapacağınız işlemin numarasını giriniz: ")

    if islem == "1":
        ad = input("Ad: ")
        soyad = input("Soyad: ")
        email = input("Email: ")
        kullanici_adi = input("Kullanıcı Adı: ")
        sifre = input("Şifre: ")
        kullanici = Kullanici(ad, soyad, email, kullanici_adi, sifre)
        film_kutuphanesi.kullanici_ekle(kullanici)

    elif islem == "2":
        ad_soyad = input("Ad ve Soyad: ")
        dogum_yili = input("Doğum Yılı: ")
        yonetmen = Yonetmen(ad_soyad, dogum_yili)
        film_kutuphanesi.yonetmen_ekle(yonetmen)

    elif islem == "3":
        film_adi = input("Film Adı: ")
        yonetmen_ad_soyad = input("Yönetmen Ad ve Soyad: ")
        cikis_yili = input("Çıkış Yılı: ")
        tur = input("Tür: ")

        yonetmen = film_kutuphanesi.yonetmen_aramak(yonetmen_ad_soyad)
        if yonetmen:
            film = Film(film_adi, yonetmen_ad_soyad, cikis_yili, tur)
            film_kutuphanesi.film_ekle(film)
        else:
            print("Girmiş olduğunuz yönetmen bilgileri bulunamadı. Lütfen öncelikle yönetmeni sisteme ekleyin daha sonrasında film eklemesi yapınız.")

    elif islem == "4":
        film_kutuphanesi.kullanicilari_goster()

    elif islem == "5":
        film_kutuphanesi.yonetmenleri_goster()

    elif islem == "6":
        film_kutuphanesi.filmleri_goster()

    elif islem == "7":
        kullanici_adi = input("Kullanıcı adını silmek için giriniz: ")
        film_kutuphanesi.kullanici_sil(kullanici_adi)

    elif islem == "8":
        yonetmen_ad_soyad = input("Yönetmen adı ve soyadını silmek için giriniz: ")
        film_kutuphanesi.yonetmen_sil(yonetmen_ad_soyad)

    elif islem == "9":
        film_adi = input("Film adını silmek için giriniz: ")
        film_kutuphanesi.film_sil(film_adi)

    elif islem == "10":
        kullanici_adi = input("Güncellemek istediğiniz kullanıcı adını giriniz: ")
        film_kutuphanesi.kullanici_guncelleme(kullanici_adi)

    elif islem == "11":
        yonetmen_ad_soyad = input("Güncellemek istediğiniz yönetmenin adını ve soyadınız giriniz: ")
        film_kutuphanesi.yonetmen_guncelleme(yonetmen_ad_soyad)

    elif islem == "12":
        film_adi = input("Güncellemek istediğiniz filmin adını giriniz: ")
        film_kutuphanesi.film_guncelleme(film_adi)

    elif islem == "13":
        film_adi = input("Bulmak istediğiniz filmin adını giriniz: ")
        film = film_kutuphanesi.film_aramak(film_adi)
        if film:
            film.bilgileri_goster()
        else:
            print("Girmiş olduğunuz film bulunamadı.")

    elif islem == "14" or islem.lower() == "q":
        print("Uygulamadan çıkış yapıldı...")
        break

    else:
        print("Lütfen geçerli bir işlem numarası giriniz.")
