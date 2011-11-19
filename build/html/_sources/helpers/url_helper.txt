##############
URL Yardımcısı
##############

URL Yardımcısı URL'ler ile çalışmanız için hazırlanmış fonksiyonları içerir.

.. contents:: Sayfa İçeriği

Yardımcının Yüklenmesi
======================

Bu yardımcı aşağıdaki kod kullanılarak yüklenebilir:

::

	$this->load->helper('url');

Kullanılabilir fonksiyonlar aşağıda listelenmiştir:

site_url()
==========

Ayar dosyanızda bulunan [config.php], sitenizin bağlantı adresini döndürür.
Site adresinizin sonuna index.php [yada ayar dosyanız index_page değişkenine
ne değer atadıysanız] ekler.

Bu fonksiyonu kullanmanız projenizin adres değişikliğinin kolayca yapılabilmesini
ve taşınabilirliğinin artmasını sağlar.

Adres sonuna eklenmesini istediğiniz kısımlar, isteğe bağlı olarak string veya 
array veritipinde gönderilebilir. Aşağıda bir örneği mevcut 

::

	echo site_url("news/local/123");

Bu örnek şuna benzer bir sonuç döndürecektir:
http://example.com/index.php/news/local/123

Buradaki örnekte ise parçaların array veritipinde nasıl verileceği gösteriliyor

::

	$segments = array('news', 'local', '123');
	echo site_url($segments);

base_url()
===========

Sitenizin bağlantı adresini ayar dosyanızda ayarlandığı gibi döndürür. Örneğin

::

	echo base_url();

Bu fonksiyon `site_url` olduğu gibi index_page veya url_suffix değerlerini adres
sonuna eklemez.

Ayrıca site_url'de olduğu gibi adresin sonuna string veya array tipinde parçacıklar 
ekletebilirsiniz.
String tipinde veri gönderim örneği

::

	echo base_url("blog/post/123");

Bu örnek şuna benzer bir sonuç döndürecektir:
http://example.com/blog/post/123

Bu faydalıdır çünkü `site_url()` fonksiyonuna benzemez. Bir dosya, resim yada stil dosyasını
bu şekilde sayfanıza dahil edebilirsiniz. Şuradaki gibi

::

	echo base_url("images/icons/edit.png");

Bu örnek şuna benzer bir sonuç döndürür:
http://example.com/images/icons/edit.png

current_url()
=============

Sitenizin o an görüntülenen kısmının tam adresini (parçacıklar dahil)
döndürür.

uri_string()
============

Bu fonksiyonun kullanılması, görüntülenen sayfanın adres parçacıklarını döndürür.
Şu örnekteki gibi adres eğer şu ise

::

	http://some-site.com/blog/comments/123

Bu fonksiyon şunu döndürür

::

	/blog/comments/123

index_page()
============

Sitenizin ayar dosyanızda tanımlanan "index" dosyasını döndürür.
Örnek:

::

	echo index_page();

anchor()
========

HTML standartlarında bir köprü oluşturabilmenizi sağlar

::

	<a href="http://example.com">Click Here</a>

Bu fonksiyon isteğe bağlı üç değer alır

::

	anchor(adres, görünecek metin, öznitelikler)

İlk değer site adresinizin sonuna eklenir. Önce site_url() fonksiyonu çağrılıp
sonuna string veya array veritipinde gönderilen değerler eklenir.

.. admonition:: Not
    :class: note
    
    Eğer alt uygulamalarınız için bağlantı oluşturacaksanız bu fonksiyonun ayar dosyanızda tanımladığınız adresi çağırdığını ve verdiğiniz değerlerin o adres sonuna ekleneceğini bilmelisiniz.

İkinci değer ise bağlantılı olarak görüntülecek metni içermelidir. Eğer boş bırakılırsa
bağlantı adresi görüntülenen metin olarak kullanılır.

Üçüncü değer ise köprü için kullanılacak öznitelikleri dahil etmenizi sağlar. Bu öznitelikler
string veritipinde olabileceği gibi array veritipinde de olabilir.

Şuraki örnekte olduğu gibi

::

	echo anchor('news/local/123', 'Haberler', 'title="Haberler başlığı"');

İşlendikten sonra: <a href="http://example.com/index.php/news/local/123"
title="Haber başlığı">Haberler</a>

::

	echo anchor('news/local/123', 'Haberler', array('title' => 'En iyi haberler!'));

İşlendikten sonra: <a href="http://example.com/index.php/news/local/123"
title="En iyi haberler!">Haberler</a>

anchor_popup()
==============

Neredeyse anchor() fonksiyonuyla aynı işi görüp tek farkı bağlantıyı 
yeni bir pencerede açmasıdır. Üçüncü değeri ile açılan pencereyi kontrol
edebilmek için JavaScript öznitelikleri gönderilebilir. Eğer üçüncü değer
boş bırakılırsa, tarayıcınız ön tanımlı ayarları ile bağlantıyı yeni bir
pencerede açar. Şurada özniteliklerle alakalı bir örnek görebilirsiniz

::

	$atts = array(               
		'width'      => '800',               
		'height'     => '600',               
		'scrollbars' => 'yes',               
		'status'     => 'yes',               
		'resizable'  => 'yes',               
		'screenx'    => '0',               
		'screeny'    => '0'             
	);

	echo anchor_popup('news/local/123', 'Tıkla!', $atts);

Uyarı: Üçüncü değeri ne yaptığınızı bilmeniz durumunda kullanmanız gerekmektedir.
Eğer JavaScript ile açılır pencerelere gönderilen değerler hakkında 
bilginiz yoksa üçüncü değeri boş bir array veritipinde değişken olarak girebilirsiniz.
Bu şekilde CodeIgniter öntanımlı ayarlarını kullanacaktır.

::

	echo anchor_popup('news/local/123', 'Click Me!', array());

mailto()
========

HTML standartlarında bir eposta bağlantı adresi oluturur. Kullanım örneği

::

	echo mailto('ben@sitem.com', 'İletişim için tıklayınız');

Ayrıca anchor() fonksiyonu gibi, üçüncü değer olarak öznitelikleri ekleyebilirsiniz.

safe_mailto()
=============

EPosta adresinizi korumak için JavaScript ile şifrelenmiş kodlar kullanıp eposta 
adres metninizi oluşturduktan sonra mailto() işlevseli gibi çalışır.Kaynak kodda oluşturduğu
JavaScript betiği eposta adresinizi spam botlarından korur.

auto_link()
===========

Otomatik olarak değer olarak vereceğiniz string içerisinde geçen eposta adresleri 
ve web adreslerini köprü haline çevirecektir. Örneğin

::

	$string = auto_link($string);

İkinci değer ise çevirmek istediğiniz türü belirtir. Belirttiğiniz tür dışında kalanlar 
çevrilmez. Burada email veya url diye vereceğiniz iki tür bulunmaktadır. Eposta bağlantıları
safe_mailto() fonksiyonunda olduğu gibi şifrelenecektir. Ön tanımlı olarak her iki türü de 
köprüleme yapmaktadır.

Sadece bağlantı adresleri

::

	$string = auto_link($string, 'url');

Sadece Eposta Adresleri

::

	$string = auto_link($string, 'email');

Üçüncü değer ise bağlantıların yeni bir pencerede gösterilip gösterilmeyeceğini belirler.
TRUE veya FALSE değerleri alabilir (boolean).

::

	$string = auto_link($string, 'both', TRUE);

url_title()
===========

Girdi olarak verilecek metinlerdeki adreslemelerde geçersiz olacak karakterlerden temizler.
Kullanıcı dostu bağlantılar oluşturmak için kullanabilirsiniz. Şurada olduğu gibi kullanılır

::

	$title = "CSS'de yanlış olan nedir ?";
	$url_title = url_title($title);  // İşlem sonunda:  CSSde-yanl-olan-nedir-

İkinci değer olarak belirteç ayarlanabilir. Öntanımlı olarak `dash` değeri tanımlıdır ve boşluk
karakteri ve diğer geçersiz karakterler yerine düz tire(-) koyar. 
Ayar olarak dash(-) veya underscore(_) kullanılabilir.

::

	$title = "CSS'de yanlış olan nedir ?";
	$url_title = url_title($title, 'underscore');  // İşlem sonunda:  CSSde_yanl_olan_nedir_

Üçüncü değer ise karakterlerde bir tümünü küçültme yapılıp yapılmayacağıdır.
Ön tanımlı değer ise yapılmayacağıdır. Boolean veritipinde değer girilebilir(TRUE/FALSE).

::

	$title = "CSS'de yanlış olan nedir ?";
	$url_title = url_title($title, 'underscore', TRUE);  // İşlem sonunda:  cssde_yanl_olan_nedir_

prep_url()
----------

Bu fonksiyon verilen bağlantının önüne http:// ekleyecektir. Şu şekilde kullanılabilir

::

	$url = "example.com";
	$url = prep_url($url);

redirect()
==========

Yönlendirme oluşturmak için kullanılabilir. Başlık(header) yönlendirme
yapmaktadır. Eğer kendi siteniz haricinde bir site yönlendirme yapacaksanız
çalışmayacaktır. Vereceğiniz değerin kendi projenizdeki bir bağlantı olması 
gereklidir ve ayar dosyasından alınan ayarların sonuna verdiğiniz değerler 
eklenerek yönlendirme gerçekleştirilir.

İsteğe bağlı ikinci değer ise bölgesel yönlendirme yöntemine zorlar. Bu
yöntemler "location" veya "refresh" olarak belirlenmiştir. "location" yöntemi 
daha hızlıdır ancak windows sunucularda daha az güvenilirdir. Öntanımlı ayarı
"auto" olmakla beraber sunucun ön tanımlı ayarını kullanır.

İsteğe bağlı üçüncü değer ise özel HTTP Cevap Kodu (HTTP Response Code) 
oluşturulmasına imkan verir. Örneğin arama motorları 301 koduna sahip sonuçları
hedefler. Öntanımlı olarak bu kod 302dir. Üçüncü değer *sadece* 'location' türünde
yönlendirmede kullanılmalıdır 'refresh' türündekilerde değil. Şuradaki şekilde::

	if ($logged_in == FALSE)
	{      
		redirect('/login/form/');
	}

	// 301 ile yönlendirme
	redirect('/article/13', 'location', 301);
	 
.. admonition:: Not
    :class: note
    
    HTTP başlıklarında en iyi kontrolü sağlamak için, 
    `Output Library </libraries/output>` set_header() fonksiyonunu kullanmalısınız.

