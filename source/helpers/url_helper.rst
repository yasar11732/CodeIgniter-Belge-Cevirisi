##########
URL Yardımcısı
##########

URL Yardımcısı URL'ler ile çalışmanız için hazırlanmış işlevseller içerir.

.. contents:: Sayfa İçeriği

Yardımcının Yüklenmesi
===================

Bu yardımcı aşağıdaki kod kullanılarak yüklenebilir:

::

	$this->load->helper('url');

Kullanılabilir işlevseller aşağıda listelenmiştir:

site_url()
==========

Ayar dosyanızda bulunan [config.php], sitenizin bağlantı adresini döndürür.
Site adresinizin sonuna index.php [yada ayar dosyanız index_page değişkenine
ne değer atadıysanız] ekler.

Bu işlevseli kullanmanız projenizin adres değişikliğinin kolayca yapılabilmesini
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

Bu işlevsel `site_url` olduğu gibi index_page veya url_suffix değerlerini adres
sonuna eklemez.

Ayrıca site_url'de olduğu gibi adresin sonuna string veya array tipinde parçacıklar 
ekletebilirsiniz.
String tipinde veri gönderim örneği

::

	echo base_url("blog/post/123");

Bu örnek şuna benzer bir sonuç döndürecektir:
http://example.com/blog/post/123

Bu faydalıdır çünkü `site_url()` işlevseline benzemez. Bir dosya, resim yada stil dosyasını
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

Bu işlevseli kullanılması, görüntülenen sayfanın adres parçacıklarını döndürür.
Şu örnekteki gibi adres eğer şu ise

::

	http://some-site.com/blog/comments/123

Bu işlevse şunu döndürür

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

Bu işlevsel isteğe bağlı üç değer alır

::

	anchor(adres, görünecek metin, öznitelikler)

İlk değer site adresinizin sonuna eklenir. Önce site_url() işlevseli çağrılıp
sonuna string veya array veritipinde gönderilen değerler eklenir.

.. note:: Eğer alt uygulamalarınız için bağlantı oluşturacaksanız bu işlevselin 
	ayar dosyanızda tanımladığınız adresi çağırdığını ve verdiğiniz değerlerin o
	adres onuna ekleneceğini bilmelisiniz.

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

Neredeyse anchor() işlevseliyle aynı işi görüp tek farkı bağlantıyı 
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

Ayrıca anchor() işlevseli gibi, üçüncü değer olarak öznitelikleri ekleyebilirsiniz.

safe_mailto()
=============

EPosta adresinizi korumak için JavaScript ile şifrelenmiş kodlar kullanıp eposta 
adres metninizi oluşturduktan sonra mailto() işlevseli gibi çalışır.Kaynak kodda oluşturduğu
JavaScript betiği eposta adresinizi spam botlarından korur.

auto_link()
===========

Automatically turns URLs and email addresses contained in a string into
links. Example

::

	$string = auto_link($string);

The second parameter determines whether URLs and emails are converted or
just one or the other. Default behavior is both if the parameter is not
specified. Email links are encoded as safe_mailto() as shown above.

Converts only URLs

::

	$string = auto_link($string, 'url');

Converts only Email addresses

::

	$string = auto_link($string, 'email');

The third parameter determines whether links are shown in a new window.
The value can be TRUE or FALSE (boolean)

::

	$string = auto_link($string, 'both', TRUE);

url_title()
===========

Takes a string as input and creates a human-friendly URL string. This is
useful if, for example, you have a blog in which you'd like to use the
title of your entries in the URL. Example

::

	$title = "What's wrong with CSS?";
	$url_title = url_title($title);  // Produces:  Whats-wrong-with-CSS

The second parameter determines the word delimiter. By default dashes
are used. Options are: dash, or underscore

::

	$title = "What's wrong with CSS?";
	$url_title = url_title($title, 'underscore');  // Produces:  Whats_wrong_with_CSS

The third parameter determines whether or not lowercase characters are
forced. By default they are not. Options are boolean TRUE/FALSE

::

	$title = "What's wrong with CSS?";
	$url_title = url_title($title, 'underscore', TRUE);  // Produces:  whats_wrong_with_css

prep_url()
----------

This function will add http:// in the event that a scheme is missing
from a URL. Pass the URL string to the function like this

::

	$url = "example.com";
	$url = prep_url($url);

redirect()
==========

Does a "header redirect" to the URI specified. If you specify the full
site URL that link will be built, but for local links simply providing
the URI segments to the controller you want to direct to will create the
link. The function will build the URL based on your config file values.

The optional second parameter allows you to force a particular redirection
method. The available methods are "location" or "refresh", with location
being faster but less reliable on Windows servers. The default is "auto",
which will attempt to intelligently choose the method based on the server
environment.

The optional third parameter allows you to send a specific HTTP Response
Code - this could be used for example to create 301 redirects for search
engine purposes. The default Response Code is 302. The third parameter is
*only* available with 'location' redirects, and not 'refresh'. Examples::

	if ($logged_in == FALSE)
	{      
		redirect('/login/form/');
	}

	// with 301 redirect
	redirect('/article/13', 'location', 301);

.. note:: In order for this function to work it must be used before anything
	is outputted to the browser since it utilizes server headers.

.. note:: For very fine grained control over headers, you should use the
	`Output Library </libraries/output>` set_header() function.
