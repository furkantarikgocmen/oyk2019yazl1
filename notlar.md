#OYK2019 Yaz Kampı - Linux Sistem Yönetimi 1. Düzey Kurs Notları
#(Eğitmenler: Aydın Doyak ve Fatih Koç)

"UNIX was not designed to stop its users from doing stupid things, as that would also stop them from doing clever things." - Doug Gwyn

---GNU Felsefesi---

0-Herhangi bir amaç için yazılımı çalıştırma özgürlüğü.
1-Her ne istiyorsanız onu yapmak için programın nasıl çalıştığını öğrenmek ve onu değiştirme özgürlüğü. Yazılımın kaynak koduna ulaşmak, bu iş için önkoşuldur.
2-Kopyaları dağıtma özgürlüğü. Böylece komşunuza yardım edebilirsiniz.
3-Tüm toplumun yarar sağlayabileceği şekilde programı geliştirme ve geliştirdiklerinizi (ve genel olarak değiştirilmiş sürümlerini) yayınlama özgürlüğü. Kaynak koduna erişmek, bunun için önkoşuldur.

#Bir yazılımın özgür yazılım olduğunu anlayabilmek için özgür yazılım lisansının olması gerekir.

#Bir özgür yazılımın satılabilmesi veya satılmaması da özgürlüğünün bir parçasıdır.

---Sistem Açılışı---

#Linux, bir işletim sistemi değil; bir işletim sistemi çekirdeğidir.

BIOS-->MBR-->GRUB-->Kernel-->Init-->Run Level Programs

Çekirdek, tüm işleri tamamladığında init prosesini başlatır. Tüm diğer servis ve programlar bu init prosesinin altında çalışır. İşlemler, dallanarak çoğaldığından; init işlemi öldürülürse tüm program ve servisler ölür.

---BASH---

Kabuk (Shell): Bilgisayar ve kullanıcı arasında diyalog sağlayan programdır.
Alternatif kabuk programları (sh, dash, zsh, fish, ksh, xiki etc...) mevcut olsa da en çok kullanılanı Bash'tir. Bash, komut satırının yorumlayıcısıdır.

#Bash, BÜYÜK-küçük harf duyarlıdır. Bu yüzden komut, parametre ve dizinleri belirtirken yazdıklarımıza dikkat etmeliyiz.

---DOSYA-DİZİN YAPISI---

Unix temelli sistemlerde anlaşılması gereken en önemli şey dosya-dizin yapısıdır.

#"Every thing is a file."

Dosya sisteminin en temelinde / ile ifade edilen kök dizin mevcuttur. Bu dizin /root ile karıştırılmamalıdır. Zira /root, root kullanıcısının ev dizinidir.

Genel olarak kök dzinin altında:
/bin (Binary Files)
/boot (Static Files of Boot Loader)
/etc (Host Specific System Config)
/usr (Shareble and Read-Only Data)
	/local (Local Software)
	/share (Static Data Shareable Among All Architectures)
		/man (Manual Pages)
	/bin (Most User Commands)
	/include (Standart Include Files for C Prog)
	/lib (obj, bin, lib files for Prog. and Packages)
	/sbin (Non-Essential Binaries)
/var (Variable Data Files)
	/cache (Application Cache Data)
	/lib (Variable State Information Remains After Reboot)
	/yp (Data for Nis Services)
	/lock (Lock Files for Shared Resources)
	/opt (Variable Data of Packages Installed)
	/run (Info of System Since It Was Booted)
	/tmp (Available for Prog.)
	/spool (Data Awaiting Processing)
	/log (Log Files and Dir)
/sbin (Sistem Binaries)
/tmp (Temperory Files Deleted on Boot Up)
/dev (Location of Special or Device Files)
/home (User Home Directories)
/lib (Library and Kernel Modules)
/mnt (Mount Files for Temperory Filesystems)
/opt (Add-On Application Software)
/root (Home Dir. for Root User)

dizinleri bulunur.

/bin dizininin altında çalıştırılabilir dosyalarımız bulunur. Herhangi bir Bash veya Python scriptini veya derlenmiş programlarımızı bu dizinin altına kopyalarsak Bash'te doğrudan çalıştırabiliriz.

/boot Grub gibi sistem açılışında gerekli olan dosyaların tutulduğu dizindir.

/etc Sisteme özel programların dosyalarının bulunduğu dizindir.

/usr (Unix System Resources) ++

++

/tmp (Temperory Files Deleted on Boot Up) Programların geçici dosyaları üretebileceği bir yer olarak belirtilmiştir. Herkes bu dosyaya yazabilir ama okunabilir kısımları izin gerektirir.

/dev (Device) Sistemimize bağlı aygıtlar.

/home Kullanıcı dosyları için ayrılmış dizin.

/lib (Library and Kernel Modules) Çekirdeğin ek programlara erişmesi için kullandığı dizindir.
#GNU Linux'ta genellikle programlar, kütüphaneleri ortak kullanırlar. Yani iki farklı program, çalışabilmek için aynı kütüphaneye ihtiyaç duyuyorsa; aynı dosyayı iki defa indirmeye gerek kalmaz.

İşlemci mimarisine göre /lib /lib32 ve /lib64 dizinleri sisteme eklenir. Dağıtıma göre farklılık gösterse de amaçları aynıdır.

/mnt Bağlanan harici diskler veya imaj dosyaları için ayrılmış dizindir. Normalde altı boştur. Sisteme yeni bir flaş bellek veya harici disk bağlamak istersek genelde buraya bağlarız.

/opt Repo dışından yüklediğimiz programlar için ayrılmış dizindir.

/root Root kullanıcısının ev dizini.

/sbin "/bin" gibi ama biraz farklı.

---Bash'te Temel Komut ve Programlar---

#man
Bash kullanırken en çok yardım alacağımız komuttur. Program ve komutların "manual" sayfalarını yani kullanım kılavuzlarını gösterir.

man [komut] şeklinde basit bir kullanımı vardır.

#ls
Dizin listelemek için kullanılır. Doğrudan "ls" yazdığımızda içinde bulunduğumuz dizin altındaki dosya ve dizinleri gösterir.

Full-Path kullanımı: ls /tam_yol/tam_yolun_devamı/

Relative-Path kullanımı: ls ../../../dizin1/dizin2/

#Full-Path (Tam Yol) Kök dizinden itibaren listelenecek olan dizinin tam yoludur.
#Relative-Path (İzafi Yol) Bulunduğumuz dizinden itibaren geri veya ileriye giderek dizinin belirtilmesidir.

Sık kullanılan parametreler:
ls -a Nokta dosyaları da göster.
ls -l Görüntülenen dizin ve dosyalar hakkında boyut ve izinler gibi detayları da göster.

ls -la şeklinde parametreleri birleştirmek de mümkündür.
Daha detaylı bilgi için man sayfasına bakınız.

# "." (Nokta) bulunduğumuz dizini ifade eder.
# ".." (İki Nokta) bulunduğumuz dizinin bir üstünü ifade eder.

#Pwd
İçinde bulunduğumuz dizini öğrenmemizi sağlar.

#history
Kabuğa girdiğimiz komut geçmişini listeler.

history 10 son 10 komutu listeler.

#history, komutlara eklediğimiz #comment'leri de gösterir.



#cd Dizin değiştirmek için kullanılır. Tek başına kullanıldığında, aktif kullanıcının ev dizinine gider.

cd .. Bir üst dizine gitmek için.

cd /full/path veya cd ../../dizin1/dizin2 şeklinde kullanılabilir.

# ~ Kulllanıcının ev dizinini ifade eder.

#clear Terminal görüntüsünü temizler. Kabuktaki hiç bir şey silinmez; sadece ekran kayar.

#date Ekrana tarih, saat ve zaman dilimi bilgisini basar.

#touch Dosyaların zaman damgalarını değiştirmeye yarar. Ancak pratik kullanımda boş dosya yaratmak için çok kullanılır.

touch yeni_dosya diyerek yeni bir dosya yaratabiliriz.
ya da
touch {1..12}.gun_notlari şeklinde bir kullanım ile 1'den 13'e kadar isimlendirilmiş yeni boş dosylaar oluşturabiliriz. Bash, işimizi kolaylaştırmak için elinden geleni yapar.

#mkdir Dizin yaratmak için kullanılır.
mkdir -p İçiçe dizin yaratmak için kullanılır.

#rm Dizin ve/veya dosya silmek için kullanılır.
rm -rf * Dizin ve dosya ne bulursan sil demektir. * yerine bir dosya veya dizin belirtebiliriz. Ya da kolay bir kullanım yolu olarak:
rm -f {1..6}.gun_notlari ile yukarıda oluşturduğumuz dosyların 7'ye kadar olanını silebiliriz.

#alias Değişken gibi komut tanımlamak için kullanılır.
alias ls="ls -ilah" komutunu verdiğimizde Bash kapanana kadar ls komutu, ilave ettiğimiz argümanlar ile çalışır. Ya da daha önce var olmayan bir isimle, komplike komutları tanımlayabiliriz.

alias kapan="/sbin/poweroff" gibi..

#alias komutu ile tanımladığımız komutu kalıcı hale getirmek için ~/.bashrc dosyasında değişiklik yapmamız gerekir.

#cat Dosyaların içeriğini gösterir (Düzenlemez! Sadece gösterir.)

#more Dosyaların içeriğini sakin sakin gösterir.

#less Dosyaların içeriğini sakin sakin gösterir ama arama yapmamıza izin verir.

#echo "mesaj" Mesajı ekrana basar.

# $? ile çıkış kodlarını görebiliriz. Ancak bu bir komut değil; değişkendir. Bu yüzden echo ile birlikte içeriğini görebiliriz. $? son çalışan programın exit kodunu tutar.

echo $? yazdığımızda 0 sonucunu alıyorsak program düzgün çalışmış ve çıkış yapmış demektir. 0 haricindeki her sonuç, bir hata mesajıdır.

---Komutları Kombine Etmek---
--
#Bu kullanımda kodlar sırayla çalışır. Aynı bash script yazar gibi ya da arka arkaya komut girer gibi.

echo deneme ; echo denemedik
deneme
denemedik
--
echo deneme && echo sesbirki && mkdir kimseyok
deneme
sesbirki

çıktısını verir ve kimseyok adında bir dizin oluşturur.
Bu kullanımın farkı, komutlardan biri hata verirse işi break eder. Yani devamındaki komutlar çalışmaz.
--
echo 1 || echo 2 || echo 3

#İlk kod çalışmazsa ikinci, o da çalışmazsa üçüncü çalışsın. Hiç olmadı çıksın.

secho 1 || echo 2 || echo 3
bash: secho: komut yok
2

çıktısını verir ve "echo 3" çalışmadan kodu bitirir.
Bunun mantıklı kullanımı şöyle olabilir:

mkdir xx || echo "olmadı"

Burada eğer mkdir kodu çalışmazsa kullanıcıya belli bir hata mesajı verebilir veya log dosyasına yazdırabiliriz.
--
#Parantezli örnek

secho 1 && echo 2 || echo 2
bash: secho: komut yok
3

secho 1 && (echo 2 || echo 3)
bash: secho: komut yok


(Ekranda 3 yazmadı.)

---Sistemden Bilgi Almak---

#uname -a Kernel sürümü, dağıtım türü, işlemci mimarisi gibi OS hakkında temel bilgileri ekrana basar.

#lsb_release İşletim sistemi hakkında bilgi almak için.

#lscpu Sistem hakkında donanım bilgisi almak için.

#lspci Bağlı aygıtları görmek için. (Sisteme bağlı olduğu halde sürücüsü yüklü olmayan veya tanınmayan aygıtlar "Unassigned Class" olarak etiketlenir.

#uptime Sistemin ayakta kalma süresini gösterir.
