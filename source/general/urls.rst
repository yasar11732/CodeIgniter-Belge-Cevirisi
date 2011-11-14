################
CodeIgniter URLs
################

URL CodeIngiter'da arama-motoru ve kullanýcý dostu varsayýlarak tasarlanmýþtýr. Standart "sorgu dizisi" (query string) yaklaþýmý yerine, dinamik sistemli eþ anlamlý, CodeIgniter **parça-tabanlý** yaklaþýmýný kullanýr::

	example.com/news/article/my_article

.. not::  Sorgu dizisi URL opsiyon olarak aþaðýda tanýmlandýðý gibi kullanýlabilir.

URI Parçalarý
============

Model-View-Controller yaklaþýmýnda URL parçalarý genelde þöyle sunulur::

	example.com/class/function/ID


#. Ýlk parça controller **sýnýfýný** hatýrlatýr.
#. Ýkinci parça sýnýftaki **fonksiyon** ya da metodu çaðýrýr.
#. Üçüncü parça ya da diðer ek parçalar, ID ile gösterilir ve controller'a deðiþken olarak gönderilir.

The :doc:`URI Sýnýfý <../libraries/uri>` ve :doc:`URL Helper <../helpers/url_helper>` dosyalarý, URI bilgilerini kolayca kullanmanýzý saðlayan fonksiyonlara sahiptir. Ek olarak, URL deðeriniz :doc:`URI Yönlendirme <routing>` özelliðiyle daha esnek bir özellik için yeniden tanýmlanabilir.

Adresten Index.php kaldýrmak
===========================

**index.php** dosyasý aþaðýdaki URL'de bulunduðunu varsayalým::

	example.com/index.php/news/article/my_article

Bu dosyayý .htaccess dosyasýnda yazacaðýnýz bir kaç basit kural ile kaldýrabilirsiniz. Burada verilen örnek dosya ile, kullanýlan çýkartma  metodu ile herþeyi yeniden yönlendirebilirsiniz:

::
	
	RewriteEngine on
	RewriteCond $1 !^(index\.php|images|robots\.txt)
	RewriteRule ^(.*)$ /index.php/$1 [L]

Yukarýdaki örnekte, index.php, images ve robots.txt dosyalarýndan farklý her HTTP istemi index.php dosyamýzý çaðýrmýþ iþlemi görecektir.

URL Soneki Eklemek
===================

**config/config.php** dosyanýzda, istediðiniz soneki ekleyerek CodeIgniter tarafýndan URL oluþturulmasýný tanýmlayabilirsiniz. Örneðin, eðer URL þöyle ise::

	example.com/index.php/products/view/shoes

Seçtiðiniz bir sonek, mesela **.html,** sayfanýzý þöyle gösterilmesini saðlar::

	example.com/index.php/products/view/shoes.html

Sorgu Dizisi Ýmkaný
======================

Bazý durumlarda URL'nizde sorgu dizisi kullanmayý tercih edebilirsiniz::

	index.php?c=products&m=view&id=345

CodeIgniter, **application/config.php** dosyasýyla bu imkaný kullanmanýza olanak tanýr. Eðer config dosyanýzý açarsanýz þunlarý göreceksiniz::

	$config['enable_query_strings'] = FALSE;
	$config['controller_trigger'] = 'c';
	$config['function_trigger'] = 'm';

Eðer "enable_query_strings" deðerini TRUE olarak deðiþtirirseniz bu özellik aktif olacaktýr.  Controller ve fonksiyonlarýnýz tetikleyici kelime geldiðinde, ayarlarsanýz controller ve metodlarýnýza ulaþabilir::

	index.php?c=controller&m=method

.. Not:: IEðer sorgu dizisi kullanacaksanýz, URL helper (ve URL üreten diðer helper, mesela bazý form helper'ý gibi) gibi parça tabanlý URL'ye uygun tasarlanmýþ helper kullanmak yerine, kendi URL'nizi inþa etmelisiniz.
