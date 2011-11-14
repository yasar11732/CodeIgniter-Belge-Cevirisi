############################
CodeIgniter Kullanım Kılavuzu
############################

*******************
Kurulum Talimatları
*******************

Codeigniter kullanım klavuzu belgeleri yönetmek ve farklı formatlarda çıktı almak 
için Sphinx kullanır.Sayfalar kolayca okunabilen `ReStructured Text <http://sphinx.pocoo.org/rest.html>`_
formatında yazılır.

Gereksinimler
=============

Sphinx, Python'a ihtiyaç duyar. Unix benzeri işletim sistemlerinde Python
zaten yüklüdür. Terminal ekranından ``python`` komutunu vererek kontrol 
edebilirsiniz. Python açılmalı ve size hangi sürümde olduğunuzu göstermeli. Eğer 2.7+ sürümünde değilseniz buyurun buradan yükleyin http://python.org/download/releases/2.7.2/

Kurulum
=======

1. Kurun `easy_install <http://peak.telecommunity.com/DevCenter/EasyInstall#installing-easy-install>`_
2. ``easy_install sphinx``
3. ``easy_install sphinxcontrib-phpdomain``
4. Kod örneklerinde PHP, HTML, CSS, ve JavaScript yazım renklendirmesi yapmaya yarayan CI Lexer yazılımını yükleyin. (bkz *cilexer/README*)
5. ``cd user_guide_src``
6. ``make html``

Belge Düzenleme ve Oluşturma
============================

Bütün kaynak dosyaları *source/* altında bulunur ve yeni belge ekleme veya
varolan belgeyi değiştirme buradan yapılır. Kendi deponuzda çevirinizi
yaptıktan sonra, bu depodaki *master* branch'ine pull isteği yapmanızı tavsiye
ederiz.

HTML nerede?
============

Aşikardır ki, HTML belgeleri en çok umursadığımız kısım, çünkü bu çeviriyi
kullanacak olanların asıl kullanacağı belgeler onlar olacak. Resmi CodeIgniter
depolarında HTML belgeleri depoya dahil edilmese de, bu depoda yapılan her
değişikten sonra HTML belgeri *build/html/* altında yeniden oluşturulacak.
Böylece html belgeleri kendi oluşturma imkanı bulamayanlar da oluşturulmuş HTML
belgelerinin nasıl göründüğü hakkında bilgi sahibi olacaklar veya doğrudan HTML
belgelerini kullanabilecekler. Bununla birlikte, yukarıda bahsedildiği gibi
Sphinx yazılımını yüklediyseniz, HTML dosyalarını oluşturmak çok basit.
Değişiklerinizi yaptıktan sonra, dosyaların kök dizininden (bu README.rst'nin
bulunduğu dizin) yükleme talimatlarındaki son komutu vermeniz yeterli::

    make html

Bir derleme aşamasından sonra, oluşturulmuş HTML dosyalarınız *build/html/*
altında oluşmuş olacak. Bu aşamadan sonra bu komutu her verdiğinizde, sadece
gerekli dosyalar yeniden oluşturulacak, böylece bir hayli zamandan kazanmış
olacaksınız.

************
Stil Rehberi
************

Sphinx kullanarak CodeIgniter belgelendirmeyle ilgili genel bir rehber için
lütfen source/documentation/index.rst'ye bakınız. Çeviri ile ilgili bir stil
rehberine de
https://github.com/yasar11732/CodeIgniter-Belge-Cevirisi/wiki/Stil-Rehberi
adresinden ulaşılabilir. 
