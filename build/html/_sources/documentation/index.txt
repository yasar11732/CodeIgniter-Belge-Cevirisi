#########################
CodeIgniter Belgesi Yazma
#########################

CodeIgniter birkaç farklı formatta belgelerini oluşturmak için, formatlamayı
üstlenmesi için reStructuredText formatı ile birlikte, Sphinx kullanır.
Eğer Mardown veya Textile kullanmışlığınız varsa, reStructuredText'i
kolaylıkla anlarsınız. Bu formatın odak noktası, okunabilirlik ve kullanım
kolaylığıdır. Oldukça teknik belgeler yazacak olsak da, bunları insanlar için
yazarız.

Bir içindekiler tablosu daima aşağıdaki gibi eklenmelidir. Bu ``.. contents::``
yönergesini kendi başına bir satıra ekleyerek oluşturulmuştur.

.. contents:: Page Contents


***************
Gerekli Araçlar
***************

HTML, ePub, PDF vb. oluşturulmuş dosyaları görmek için Sphinx ve Sphinx için
PHP domain eklentisini de kurmanız gerekiyor. Altta yatan bir gereksinim ise
Python'un kurulu olması. Son olarak, Pygments için CI Lexer'ı yüklemeniz
gerek. Böylece kod blokları düzgün bir şekilde renklendirilecek.

.. code-block:: bash

	easy_install sphinx
	easy_install sphinxcontrib-phpdomain

Daha sonra :samp:`cilexer` klasöründeki README dosyasındaki yönergeleri takip
ederek CI Lexer'ı kurun.



****************************************
Sayfa, Bölüm Başlıkları ve Alt Başlıklar
****************************************

Başlıklar sayfa içerisinde sıra ve bölümleme oluşturmakla kalmaz, aynı zamanda
otomatik olarak hem sayfa, hem de belge için *içindekiler* oluşturuken
kullanılır. 

Başlıklar bir kısım yazı için belirli karakterleri altçizgi olarak kullanarak
oluşturulur. Sayfa başlıkları ve bölüm başlıkları gibi büyük başlıklar aynız
zamanda üstçizgi de kullanır. Diğer başlıklar aşağıdaki sıralamada haliyle sadece alt çizgi kullanır::

	# üstçizgi ile birlikte, sayfa başlığı için
	* üstçizgi ile birlikte, büyük bölüm başlığı için
	= altbölüm için
	- altaltbölüm için
	^ altaltaltbölüm için
	" altaltaltaltbölüm için (!)
	
:download:`TextMate ELDocs Bundle <./ELDocs.tmbundle.zip>` aşağıdaki tab
tetikleyicileri ile bunları kolayca oluşturmanıza yardımcı olabilir::

	title->
	
		##########
		Page Title
		##########

	sec->
	
		*************
		Major Section
		*************
		
	sub->
	
		Subsection
		==========
		
	sss->
	
		SubSubSection
		-------------
		
	ssss->
	
		SubSubSubSection
		^^^^^^^^^^^^^^^^
		
	sssss->
	
		SubSubSubSubSection (!)
		"""""""""""""""""""""""




*******************
Metot Belgelendirme
*******************

When documenting class methods for third party developers, Sphinx provides
directives to assist and keep things simple.  For example, consider the following
ReST:

.. code-block:: rst

	.. php:class:: Some_class

	some_method()
	=============

		.. php:method:: some_method ( $foo [, $bar [, $bat]])

			This function will perform some action. The ``$bar`` array must contain
			a something and something else, and along with ``$bat`` is an optional
			parameter.

			:param int $foo: the foo id to do something in
			:param mixed $bar: A data array that must contain aa something and something else
			:param bool $bat: whether or not to do something
			:returns: FALSE on failure, TRUE if successful
			:rtype: Boolean
		
			::

				$this->load->library('some_class');

				$bar = array(
					'something'		=> 'Here is this parameter!',
					'something_else'	=> 42
				);

				$bat = $this->some_class->should_do_something();

				if ($this->some_class->some_method(4, $bar, $bat) === FALSE)
				{
					show_error('An Error Occurred Doing Some Method');
				}
		
			.. note:: Here is something that you should be aware of when using some_method().
					For real.
					
			See also :php:meth:`Some_class::should_do_something`

	should_do_something()
	=====================

		.. php:method:: should_do_something()

			:returns: whether or something should be done or not
			:rtype: Boolean


It creates the following display:

.. php:class:: Some_class

some_method()
=============

	.. php:method:: some_method ( $foo [, $bar [, $bat]])

		This function will perform some action. The ``$bar`` array must contain
		a something and something else, and along with ``$bat`` is an optional
		parameter.

		:param int $foo: the foo id to do something in
		:param mixed $bar: A data array that must contain aa something and something else
		:param bool $bat: whether or not to do something
		:returns: FALSE on failure, TRUE if successful
		:rtype: Boolean
		
		::

			$this->load->library('some_class');

			$bar = array(
				'something'		=> 'Here is this parameter!',
				'something_else'	=> 42
			);

			$bat = $this->some_class->should_do_something();

			if ($this->some_class->some_method(4, $bar, $bat) === FALSE)
			{
				show_error('An Error Occurred Doing Some Method');
			}
		
		.. note:: Here is something that you should be aware of when using some_method().
				For real.
				
		See also :php:meth:`Some_class::should_do_something`

should_do_something()
=====================

	.. php:method:: should_do_something()

		:returns: whether or something should be done or not
		:rtype: Boolean
