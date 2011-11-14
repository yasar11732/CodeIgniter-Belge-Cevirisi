###########
Controllers
###########

Controllers uygulamanızın kalbidir, yani HTTP isteklerinin nasıl ele alınması 
gerektiğini belirlemek gibi.

.. contents:: Sayfa İçeriği

Controller nedir ?
==================

**Controller, sadece bir URI ile ilişkili bir şekilde isim alan sınıf 
dosyasıdır.**

Bu URI'a göz atalaım::

	example.com/index.php/blog/

Yukarıdaki örnekte, CodeIgniter "blog.php" isimli dosyayı bulmayı ve yüklemeyi 
dener.


**Controller'ın adı bir URI'ın ilk segmenti eşleştiğinde, yüklenecektir.**


Hadi deneyelim: Merhaba Dünya !
===============================

Basit bir controller oluşturalım ve yaptıklarımıza göz atalım. Yazı editörünüzü 
kullanarak,"blog.php" adlı bir sayfa oluşturun, ve altına bu kodları yazın::

	<?php
	class Blog extends CI_Controller {

		public function index()
		{
			echo 'Merhaba Dünya !';
		}
	}
	?>

Dosyanızı "application/controllers/" klasörünüzün içine kaydedin.

Şimdi buna benzer bir URL ile sitenizi ziyaret edin::

	example.com/index.php/blog/

Eğer doğru yaptıysanız, "Merhaba Dünya !" yazısını görüyor olmalısınız.

Not: Sınıf isimleri herzaman büyük harf ile başlamalıdır.::

	<?php
	class Blog extends CI_Controller {

	}
	?>
	

Bu örnek geçerli **değildir**::

	<?php
	class blog extends CI_Controller {

	}
	?>

Ayrıca bir Controller tüm işlevlerini herzaman "CI_Controller" sınıfından 
devralır.

Fonksiyonlar
============

Yukarıdaki örnekteki fonksiyonun ismi index(). "index" fonksiyonu
herzaman varsayılan olarak yüklenir tabi URI'ın **ikinci bölümü**
boşsa. "Merhaba Dünya" mesajını göstermenin başka bir yolu şudur::

	example.com/index.php/blog/index/

**URI'ın ikinci bölümü, controller'daki hangi fonksiyonun
çağırılacağını belirler.**

Hadi deneyelim. "blog.php" dosyanıza yeni bir fonksiyon ekleyin::

	<?php
	class Blog extends CI_Controller {

		public function index()
		{
			echo 'Merhaba Dünya !';
		}

		public function mesajlar()
		{
			echo 'Şuna bak !';
		}
	}
	?>

Şimdi aşağıdaki URL'i girerek "mesajlar" fonksiyonunu görelim::

	example.com/index.php/blog/mesajlar/

Yeni mesajınızı görüyor olmalısınız.

Fonksiyonumuza URI Bölümü Ekleyelim
===================================

Eğer URI'ınız 2 bölümden fazlasını içeriyorsa bu bölümler fonksiyonunuza
parametre olarak geçer.

Örnek olarak, diyelim ki şuna benzer bir URI'ınız var::

	example.com/index.php/urunler/ayakkabilar/sandaletler/123

Fonksiyonunuz URI'ın 3. ve 4. bölümlerini parametre olarak
alacak ("sandaletler" and "123")::

	<?php
	class Urunler extends CI_Controller {

	    public function ayakkabilar($sandaletler, $id)
	    {
	        echo 'İlk parametre : '.$sandaletler;
	        echo 'İkinci parametre : '.$id;
	    }
	}
	?>

.. important:: Eğer :doc:`URI Yönlendirme <routing>` özelliğini kullanıyorsanız,
	bu bölümler fonksiyonunuza yeniden yönlendirilebilir olarak geçecektir.

Öntanımlı Controller'ı Tanımlamak
=================================

CodeIgniter bir URI mevcut değilken bile ön tanımlı olan bir Controller
yükleyebilir. Sadece sitenizin kök URL'i sabit olacak.
Ön tanımlı Controller'ı yüklemek için, **application/config/routes.php**
dosyanızı açın ve şu değişkeni ayarlayın::

	$route['default_controller'] = 'Blog';

"Blog" kullanılmasını istediğiniz sınıfın adıdır. Eğer şimdi ana "index.php"
dosyanızı URI belirtmeden yüklerseniz ön tanımlı olarak "Merhaba Dünya !"
mesajını göreceksiniz.

Remapping Fonksiyonunu Çağırmak
===============================

Yukarıda belirtildiği gibi, URI'ın ikinci bölümü normal olarak hangi
fonksiyonun Controller olarak çağırılacağını belirler. CodeIgniter
_remap() fonksiyonu ile bu davranışı geçersiz kılmanıza izin verir::

	public function _remap()
	{
	    // Some code here...
	}

.. important:: Eğer Controller'ınız _remap() isimli bir fonksiyon içeriyorsa,
	URI içeriği ne olursa olsun herzaman denileni alacaktır.
	Normal davranış olan URI içeriğinin hangi fonksiyonu çağıracağını
	belirlemesini geçersiz kılar ve kendi kurallarınızı belirlemenize izin verir

Geçersiz kılınmış bir fonksiyon çağrısı (URI'ın ikinci bölümü)
_remap() fonksiyonuna parametre olarak geçirilir::

	public function _remap($method)
	{
	    if ($method == 'bir_method')
	    {
	        $this->$method();
	    }
	    else
	    {
	        $this->default_method();
	    }
	}

Any extra segments after the method name are passed into _remap() as an
optional second parameter. This array can be used in combination with
PHP's `call_user_func_array <http://php.net/call_user_func_array>`_
to emulate CodeIgniter's default behavior.

::

	public function _remap($method, $params = array())
	{
	    $method = 'process_'.$method;
	    if (method_exists($this, $method))
	    {
	        return call_user_func_array(array($this, $method), $params);
	    }
	    show_404();
	}

Processing Output
=================

CodeIgniter has an output class that takes care of sending your final
rendered data to the web browser automatically. More information on this
can be found in the :doc:`Views <views>` and :doc:`Output class <../libraries/output>` pages. In some cases, however, you
might want to post-process the finalized data in some way and send it to
the browser yourself. CodeIgniter permits you to add a function named
_output() to your controller that will receive the finalized output
data.

.. important:: If your controller contains a function named _output(),
	it will **always** be called by the output class instead of echoing the
	finalized data directly. The first parameter of the function will
	contain the finalized output.

Here is an example::

	public function _output($output)
	{
	    echo $output;
	}

.. note:: Please note that your _output() function will receive the data in its
	finalized state. Benchmark and memory usage data will be rendered, cache
	files written (if you have caching enabled), and headers will be sent
	(if you use that :doc:`feature <../libraries/output>`) before it is
	handed off to the _output() function.
	To have your controller's output cached properly, its _output() method
	can use::

		if ($this->output->cache_expiration > 0)
		{
		    $this->output->_write_cache($output);
		}

	If you are using this feature the page execution timer and memory usage
	stats might not be perfectly accurate since they will not take into
	acccount any further processing you do. For an alternate way to control
	output *before* any of the final processing is done, please see the
	available methods in the :doc:`Output Class <../libraries/output>`.

Private Functions
=================

In some cases you may want certain functions hidden from public access.
To make a function private, simply add an underscore as the name prefix
and it will not be served via a URL request. For example, if you were to
have a function like this::

	private function _utility()
	{
	  // some code
	}

Trying to access it via the URL, like this, will not work::

	example.com/index.php/blog/_utility/

Organizing Your Controllers into Sub-folders
============================================

If you are building a large application you might find it convenient to
organize your controllers into sub-folders. CodeIgniter permits you to
do this.

Simply create folders within your application/controllers directory and
place your controller classes within them.

.. note:: When using this feature the first segment of your URI must
	specify the folder. For example, lets say you have a controller located
	here::

		application/controllers/products/shoes.php

	To call the above controller your URI will look something like this::

		example.com/index.php/products/shoes/show/123

Each of your sub-folders may contain a default controller which will be
called if the URL contains only the sub-folder. Simply name your default
controller as specified in your application/config/routes.php file

CodeIgniter also permits you to remap your URIs using its :doc:`URI
Routing <routing>` feature.

Class Constructors
==================

If you intend to use a constructor in any of your Controllers, you
**MUST** place the following line of code in it::

	parent::__construct();

The reason this line is necessary is because your local constructor will
be overriding the one in the parent controller class so we need to
manually call it.

::

	<?php
	class Blog extends CI_Controller {

	       public function __construct()
	       {
	            parent::__construct();
	            // Your own constructor code
	       }
	}
	?>

Constructors are useful if you need to set some default values, or run a
default process when your class is instantiated. Constructors can't
return a value, but they can do some default work.

Reserved Function Names
=======================

Since your controller classes will extend the main application
controller you must be careful not to name your functions identically to
the ones used by that class, otherwise your local functions will
override them. See :doc:`Reserved Names <reserved_names>` for a full
list.

That's it!
==========

That, in a nutshell, is all there is to know about controllers.
