#################
Helper Fonksiyonu
#################

Helper'lar, isimlerinden de anlaşıldığı gibi, size yardımcılardır. Her helper dosyası belli kategorilerde fonskiyonları basitçe toparlar. **URL Helper** link oluşturmada yardımcı olur, **Form Helper** form oluşturmaya yardımcı olur, **Text Helper** metin formatı rutinlerini gerçekleştirir, **Cookie Helper** dosyaları cookie bilgilerini ayarlar ve okur, **File Helper** dosya işlemlerine yardım eder, vs.

CodeIgniter'daki diğer sistemlerden farkı, Helper'lar nesne tabanı formatında yazılmamışlardır. Basit ve prosedürel fonsiyonlardır. Her helper fonksiyonu bir görev için diğer fonksiyonlara bağlı olamdan çalışır.

CodeIgniter Helper dosyalarını açılışta yüklemez, bu yüzden ilk adım Helper dosyasını yüklemektir. Bir kez yüklendiğinde, :doc:`controller <../general/controllers>` ve :doc:`views <../general/views>` dosyalarında kullanılır hale gelir.

Helper'lar tipik olarak **system/helpers** dizini altında ya da **system/application/helpers** dizini altına yerleştirilmiştir. CodeIgniter ilk önce **system/application/helpers** dizini altını kontrol eder. Eğer dizin altında yoksa ya da tanımlanan helper bu sizinde değilse, CI genel system/helpers dizini altını kontrol eder.

Helper Yüklemek
===============

Aşağıdaki fonksiyon kullanarak helper yüklemek oldukça basittir::

	$this->load->helper('name');

Burada **name**, helper'ın adıdır; .php dosya uzantısı ya da "helper" kısmı olmadan kullanılır.

Örneğin, **url_helper.php** isimli **URL Helper**'ını yüklemek için, şunu yapmalısınız::

	$this->load->helper('url');

Helper dosyası kullanmadan önce controller fonksiyonlarının herhangi bir yerinde (hatta iyi bir pratik olmamasına karşın View dosyalarında bile) çağrılır. Helper'ı controller'ın constructor kısmında yüklerseniz, o controller'ın bütün fonksiyonlarında geçerli olur ya da istediğiniz bir fonksiyonda yükleyebilirsiniz.

Not: Helper'lar geri değer döndürmezler o nedenle bir değişkene atamayı denemeyin. Sadece kullanın.

Çoklu Helper Yüklemek
=====================

Eğer birden fazla helper'i aynı anda yüklemeniz gerekliyse, onları bir diziye atın, şunun gibi::

	$this->load->helper( array('helper1', 'helper2', 'helper3') );

Helper'ları Otomatik Yüklemek
=============================

Eğer bütün uygulamanızda kullanmanız gereken özel bir helper varsa, bunu CodeIgniter'a sistem başlarken söyleyebilirsiniz. Bunun için **application/config/autoload.php** dosyasını açmalı ve helper'ı otomatik yükleme dizisine eklemelisiniz.

Helper Kullanmak
================

Bir defa yüklendiğinde, Helper dosyasındaki fonksiyonları, standart PHP fonksiyonu gibi niyetlendiğiniz yerde kullanabilirsiniz.

Örneğin, anchor() fonskiyonu kullanarak bir view dosyanızda link oluşturmak için şunu yapmalısınız::

	<?php echo anchor('blog/comments', 'Click Here');?>

Burada "Click Here" linkin adı ve "blog/comments" ise link vermek istediğini controller/fonskiyon URI bilgisidir.

Helper'ı "Genişletmek"
======================

Helper'ı "genişletmek", mevcut dosyası olan bir helper'ı **application/helpers/** dizini altında, **MY_** önekiyle değiştirmek (bu maddeye ayar yapılabilir. Aşağıya bakınız.).

Eğer bütün ihtiyacınız mevcut helper'a biraz fonksiyonellik kazandırmaksa -belki bir iki fonksiyon eklemek ya da belirli bir fonksiyonu değiştirmekse-, kendi fonksiyonlarınız mevcut fonskiyonların yerine geçecektir. Bu durumda en iyisi basitçe helper'ı "genişletmek"tir. "Genişletmek" terimi, helper fonksiyonları usulüne ait, münferit olması ve geleneksel programlamadaki algı gibi genişletilmemesi nedneiyle gevşek kullanılır. Bu başlık altında, bu bilgiler size Helper'a yeni fonksiyon eklemek ya da mevcut helper fonksiyonlarını değiştirme yetkisi verir.

Örneğin, **Array Helper**'ı genişletmek için, **application/helpers/** dizini altında **MY_array_helper.php** dosyası oluşturun ve aşağıdaki fonksiyonu üzerine yazın ya da ekleyin::

	// any_in_array() is not in the Array Helper, so it defines a new function
	function any_in_array($needle, $haystack)
	{
	    $needle = (is_array($needle)) ? $needle : array($needle);

	    foreach ($needle as $item)
	    {
	        if (in_array($item, $haystack))
	        {
	            return TRUE;
	        }
	        }

	    return FALSE;
	}

	// random_element() is included in Array Helper, so it overrides the native function
	function random_element($array)
	{
	    shuffle($array);
	    return array_pop($array);
	}

Kendi Önekinizi Ayarlamak
-------------------------

"Genişletilmiş" Helper için dosya öneki genişletimiş kütüphaneler ve çekirdek sınıflar için aynıdır. Kendi önekinizi kullanmak için, **application/config/config.php** dosyasını açın ve şunu arayın::

	$config['subclass_prefix'] = 'MY_';

Lütfen dikkat, **CI\_** öneki CodeIgniter kütüphanelerinde kullanılır bu nedenle kendi önekiniz olarak KULLANMAYIN.	

Şimdi Ne Yapalım?
=================

İçindekiler Tablosunda mevcut Helper dosyalarının listesini bulacaksınız. Herbirini araştırın ve ne işe yaradığına bakın.
