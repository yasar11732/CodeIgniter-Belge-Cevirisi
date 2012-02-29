##################
CodeIgniter'da URL
##################

URL CodeIngiter'da arama-motoru ve kullanıcı dostu varsayılarak tasarlanmıştır. Standart "sorgu dizisi" (query string) yaklaşımı yerine, dinamik sistemli eş anlamlı, CodeIgniter **parça-tabanlı** yaklaşımını kullanır::

	example.com/news/article/my_article

.. admonition:: Not
    :class: note

    Sorgu dizisi URL opsiyon olarak aşağıda tanımlandığı gibi kullanılabilir.

URI Parçaları
=============

Model-View-Controller yaklaşımında URL parçaları genelde şöyle sunulur::

	example.com/class/function/ID


#. İlk parça controller **sınıfını** hatırlatır.
#. İkinci parça sınıftaki **fonksiyon** ya da metodu çağırır.
#. Üçüncü parça ya da diğer ek parçalar, ID ile gösterilir ve controller'a değişken olarak gönderilir.

:doc:`URI Sınıfı <../libraries/uri>` ve :doc:`URL Helper <../helpers/url_helper>` dosyaları, URI bilgilerini kolayca kullanmanızı sağlayan fonksiyonlara sahiptir. Ek olarak, URL değeriniz :doc:`URI Yönlendirme <routing>` özelliğiyle daha esnek bir özellik için yeniden tanımlanabilir.

Adresten Index.php kaldırmak
============================

**index.php** dosyası aşağıdaki URL'de bulunduğunu varsayalım::

	example.com/index.php/news/article/my_article

If your Apache server has mod_rewrite enabled, you can easily remove this
file by using a .htaccess file with some simple rules. Here is an example
of such a file, using the "negative" method in which everything is redirected
except the specified items:

::
	
	RewriteEngine On
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule ^(.*)$ index.php/$1 [L]

Yukarıdaki örnekte, var olmayan her dosya veya klasör için yapılan HTTP istemi index.php dosyamızı çağırmış işlemi görecektir.

URL Soneki Eklemek
==================
.. note:: Note: These specific rules might not work for all server configurations.

**config/config.php** dosyanızda, istediğiniz soneki ekleyerek CodeIgniter tarafından URL oluşturulmasını tanımlayabilirsiniz. Örneğin, eğer URL şöyle ise::

	example.com/index.php/products/view/shoes

Seçtiğiniz bir sonek, mesela **.html,** sayfanızı şöyle gösterilmesini sağlar::

	example.com/index.php/products/view/shoes.html

Sorgu Dizisi İmkanı
===================

Bazı durumlarda URL'nizde sorgu dizisi kullanmayı tercih edebilirsiniz::

	index.php?c=products&m=view&id=345

CodeIgniter, **application/config.php** dosyasıyla bu imkanı kullanmanıza olanak tanır. Eğer config dosyanızı açarsanız şunları göreceksiniz::

	$config['enable_query_strings'] = FALSE;
	$config['controller_trigger'] = 'c';
	$config['function_trigger'] = 'm';

Eğer "enable_query_strings" değerini TRUE olarak değiştirirseniz bu özellik aktif olacaktır.  Controller ve fonksiyonlarınız tetikleyici kelime geldiğinde, ayarlarsanız controller ve metodlarınıza ulaşabilir::

	index.php?c=controller&m=method

.. admonition:: Not
    :class: note

    Eğer sorgu dizisi kullanacaksanız, URL helper (ve URL üreten diğer helper, mesela bazı form helper'ı gibi) gibi parça tabanlı URL'ye uygun tasarlanmış helper kullanmak yerine, kendi URL'nizi inşa etmelisiniz.
