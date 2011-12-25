##########
FTP Sınıfı
##########

CodeIgniter 'ın FTP sınıfı sunucuya dosya yüklenmesine izin verir. Yüklenen dosyalar taşınabilir, ismi değiştirilebilir ve silinebilirler. Ayrıca, FTP sınıfının yerel dizini sunucu üzerindeki dizini, sunucu üzerine aynalayan fonksiyona ("mirroring") sahiptir.

.. not:: SFTP ve SSL FTP protokolleri desteklenmes. Sadece sstandart FTP protokolü desteklenir.

********************
Sınıfın Başlatılması
********************

CodeIgniter'daki diğer sınıflar gibi, FTP sınıfı da controller dosyasında $this->load->library fonksiyonu kullanılarak başlatılır::

	$this->load->library('ftp');

Bir kere yüklendikten sonra, FTP objesi şöyle kullanılır: $this->ftp

Kullanım Örnekleri
==================

Bu örnekte, FTP bağlantısı sunucu üzerine açılır ve yerel dosyalar okunur, ASCII mod ile yüklenir. Dosya izinleri 755 olarak ayarlanmıştır. Not: İzin ayarları PHP 5 için şarttır.

::

	$this->load->library('ftp');

	$config['hostname'] = 'ftp.example.com';
	$config['username'] = 'your-username';
	$config['password'] = 'your-password';
	$config['debug']	= TRUE;

	$this->ftp->connect($config);

	$this->ftp->upload('/local/path/to/myfile.html', '/public_html/myfile.html', 'ascii', 0775);

	$this->ftp->close();

Bu örnekte, sunucu üzerindeki dosyaların listesi alınır.
::

	$this->load->library('ftp');

	$config['hostname'] = 'ftp.example.com';
	$config['username'] = 'your-username';
	$config['password'] = 'your-password';
	$config['debug']	= TRUE;

	$this->ftp->connect($config);

	$list = $this->ftp->list_files('/public_html/');

	print_r($list);

	$this->ftp->close();

Bu örnekte, yerel dizin sunucu üzerine aynalanır.	
	
::

	$this->load->library('ftp');

	$config['hostname'] = 'ftp.example.com';
	$config['username'] = 'your-username';
	$config['password'] = 'your-password';
	$config['debug']	= TRUE;

	$this->ftp->connect($config);

	$this->ftp->mirror('/path/to/myfolder/', '/public_html/myfolder/');

	$this->ftp->close();

*******************
Fonksiyon Referansı
*******************

$this->ftp->connect()
=====================

FTP sunucuya bağlanır ve kayıt bilgisi tutulur. Bağlantı tercihleri fonksiyona bir dizi ile aktarılabilidiği gibi config dosyası içinde de aktarılabilir.

Bu örnekte, tercihlerin dizi ile nasıl aktarıldığı gösterilmektedir::

	$this->load->library('ftp');

	$config['hostname'] = 'ftp.example.com';
	$config['username'] = 'your-username';
	$config['password'] = 'your-password';
	$config['port']     = 21;
	$config['passive']  = FALSE;
	$config['debug']    = TRUE;

	$this->ftp->connect($config);

FTP Tercihlerinin Config Dosyası ile Ayarlanması
************************************************

Eğer FTP tercihlerinizi kayıt etmek isterseniz bir config dosyası hazırlamalısınız. Hızlıca, ftp.php isimli bir dosya oluşturun, bu dosya içine $config isimli dizi ekleyin. Daha sonra bu dosyayı config/ftp.php yoluna kayıt ederseniz, bu dosya otomatik olarak kullanılır.

Kullanılabilen bağlantı opsiyonları
***********************************

-  **hostname** - FTP sunucu adı. Genelde şunun gibidir:
   ftp.example.com
-  **username** - FTP kullanıcı adı.
-  **password** - FTP şifresi.
-  **port** - Port numarası. Varsayılan 21 dir.
-  **debug** - TRUE/FALSE (boolean). Hata mesajları gösterilsin/gösterilmesin
-  **passive** - TRUE/FALSE (boolean). Passive mod kullanılırsa. Passive mod varsayılır.

$this->ftp->upload()
====================

Sunucuya bir dosya yükler. Yerel dosya yolu ile sunucudaki dosya yolunu tanımlamalısınız. Yükleme modunu ve izin değeri opsiyonel verilebilir.
Örnek::

	$this->ftp->upload('/local/path/to/myfile.html', '/public_html/myfile.html', 'ascii', 0775);

**Mod opsiyonları:** ascii, binary, and auto (varsayılan). Eğer auto değeri kullanılırsa, kaynak dosyanın dosya uzantısı yükleme sırasında baz olarak kullanılır.

PHP 5 kullanıyorsanız, izinler geçerlidir ve dördüncü parametrede octal değer olarak aktarılır.

$this->ftp->download()
======================

Sunucudan bir dosya indirir. Sunucu üzerinde dosya yolu ile yerel dosya yolunu tanımlamalısınız, mod değeri opsiyonel verilebilir. Örneğin::

	$this->ftp->download('/public_html/myfile.html', '/local/path/to/myfile.html', 'ascii');

**Mod opsiyonları:** ascii, binary, and auto (varsayılan). Eğer auto değeri kullanılırsa, kaynak dosyanın dosya uzantısı yükleme sırasında baz olarak kullanılır.

Dosya indirilmesi gerçekleşmezse (eğer PHP yerel dosyayı yazma yetkisi yoksa da) geri dönüşü FALSE değeridir. 

$this->ftp->rename()
====================

Dosya ismini değiştirir. Dosya adı/yolu ile yeni dosya adı/yolu değeri tanımlanır.

::

	// Renames green.html to blue.html
	$this->ftp->rename('/public_html/foo/green.html', '/public_html/foo/blue.html');

$this->ftp->move()
==================

Dosyayı taşır. Kaynak ve hedef yol değeri tanımlanmalıdır::

	// Moves blog.html from "joe" to "fred"
	$this->ftp->move('/public_html/joe/blog.html', '/public_html/fred/blog.html');

Not: Eğer hedef dosya adı farklı ise dosyanın ismi değiştirilir.

$this->ftp->delete_file()
==========================

Dosya siler. Kaynak yolu ve dosya ismi tanımlanmalıdır::

	 $this->ftp->delete_file('/public_html/joe/blog.html');

$this->ftp->delete_dir()
=========================

Dizini ve altındaki herşeyi siler. Dizin adı ve kaynak yolu sonuna ters bölü (slash) ile tanımlanmalıdır.

**Önemli** bu fonksiyonu kullanırken ÇOK DİKKATLİ olun. Kaynak yolun altındaki **herşey**, alt dizinler ve dosyalar dahil silinebilir. Yolun geçerli olduğundan emin olun. Yolun doğruluğundan emin olmak için önce list_files() komutunu çalıştırın.

::

	 $this->ftp->delete_dir('/public_html/path/to/folder/');

$this->ftp->list_files()
=========================

Sunucunuz altındaki dosyaların listesini dizin olarak geri gönderir. Liste almak isteiğiniz dizinin yolunu tanımlamalısınız.

::

	$list = $this->ftp->list_files('/public_html/');

	print_r($list);

$this->ftp->mirror()
====================

Yerel dizin altındaki (tüm alt dizinler dahil) yolu okur ve sunucu üzerine FTP kullanarak aynalar. Orijinal dosya yolunun altındaki dizin yapısında ne varsa sunucu üzerine yeniden oluşturur. Kaynak yolunu ve hedef yolunu tanımlamalısınız::

	 $this->ftp->mirror('/path/to/myfolder/', '/public_html/myfolder/');

$this->ftp->mkdir()
===================

Sunucunuz üzerine dizin oluşturur. Oluşturmak istediğiniz dosya adını sonuna ters bölü işareti kullanarak tanımlamalısınız. İzinler (eğer PHP 5 kullanıyorsanız), ikinci parametre olarak octal formatta aktarılmalıdır.

::

	// Creates a folder named "bar"
	$this->ftp->mkdir('/public_html/foo/bar/', DIR_WRITE_MODE);

$this->ftp->chmod()
===================

Dosya izinlerini değiştirir. İzinlerini değiştirmek istediğiniz dosya adını ve yolunu tanımlamalısınız::

	// Chmod "bar" to 777
	$this->ftp->chmod('/public_html/foo/bar/', DIR_WRITE_MODE);

$this->ftp->close();
====================

Sunucunuzla bağlantıyı kapatır. Yükleme işlemini bitirince kapatmanız önerilir.