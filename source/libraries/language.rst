##########
Dil sınıfı
##########

Dil sınıfı, sitenizin birden fazla dile sahip olabilmesi için, dil dosyalarını ve istediğiniz dile ait satırları yüklemenizi sağlar.

Codeigniter sistem klasörlerinizde içinde dil dosyaları bulunan language isimli bir klasör bulacaksınız. Hata mesajlarını veya istediğiniz diğer mesajları göstermeniz için gereken kendi dil dosyalarınızı burada oluşturabilirsiniz.

Dil dosyaları tipik olarak system/language klasöründe bulunur. Alternatif olarak application klasörünüzü içinde language isimli bir klasör oluşturup, dil dosyalarınızı orada da bulundurabilirsiniz. Codeigniter ilk olarak system/application/language klasörünüze bakacaktır. Eğer bu klasör yoksa veya aradığı dil dosyası burada değilse, codeigniter global system/language klasörünüzü kontrol edecektir.

.. not:: Her dil dosyası kendine ait bir klasörde bulunmalıdır. Örneğin, İngilizce dil dosyaları şuradadır: system/language/english

Dil Dosyası Oluşturma
=====================

Dil dosyaları uzantı olarak _lang.php ile isimlendirilmelidir. Örneğin, diyelim ki hata mesajlarını içeren bir dil dosyası oluşturacaksınız.hata_lang.php olarak isimlendirebilirsiniz.

Dosyanın içinde, her satırı $lang isimli bir diziye şu şekilde atayacaksınız::

	$lang['language_key'] = "The actual message to be shown";

.. not:: Diğer dosyalardaki benzer isimlerle çakışma yaşanmaması için çalıştığınız dosyadaki tüm anahtar kelimelerinize ortak bir önek getirmek güzel bir alışkanlıktır. Örneğin, eğer hata mesajları oluşturuyorsanız, onlara error\_ öneki getirebilirsiniz.

::

	$lang['error_email_missing'] = "You must submit an email address";
	$lang['error_url_missing'] = "You must submit a URL";
	$lang['error_username_missing'] = "You must submit a username";

Dil Dosyası Yükleme
===================

Belli bir dosyadan istediğiniz satırı alabilmek için, önce dil dosyasını yüklemelisiniz. Dil dosyası şöyle yüklenir::

	$this->lang->load('filename', 'language');

dosyaadı yüklemek istediğiniz dosyanın adını (dosya öneki olmadan), ve dil o dosyayı içeren dil setini belirtir (örneğin, turkce). Eğer ikinci parametre eksikse, application/config/config.php dosyanızda önceden tanımladığınız dil seti yüklenir.

Bir Yazı Satırı Almak
=====================

Arzu ettiğiniz dil dosyası bir kere yüklendiğinde, bu fonksiyonu kullanarak herhangi bir satıra ulaşabilirsiniz::

	$this->lang->line('language_key');

language_key göstermek istediğiniz satıra ait array anahtarına karşılık gelir.

Note: Bu işlev sadece satırı döndürür. Ekrana basmaz.

Dil Satırlarını Form Etkiketi Olarak Kullanma
---------------------------------------------

Bu özellik artık dil sınıfından :doc:`Dil yardımcısının <../helpers/language_helper>` lang() fonksiyonuna taşınmıştır .


Dilleri Otomatik Yükleme
========================

Eğer belirli bir dili uygulamanızın tümünde kullanma ihtiyacınız varsa , Codeigniter'a sistem başlangıcında :doc:`oto-yükleme <../general/autoloader>` yapmasını isteyebilirsiniz. Bunu application/config/autoload.php dosyasını açıp,istediğiniz dil(ler)i autoload arrayine ekleyerek yapabilirsiniz.
