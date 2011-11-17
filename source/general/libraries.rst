######################################
CodeIgniter Kütüphanelerinin Kullanımı
######################################

Bütün mevcut kütüphaneler system/libraries dizini altındadır. Çoğu zaman, kütüphanelerin kullanıması için :doc:`controller <controllers>` dosyasında aşağıdaki fonksiyon ile başlatmak gerekir::

	$this->load->library('class name'); 

Burada class name başlatmak istediğiniz sınıfın adıdır. Örneğin, doğrulama (validation) sınıfını başlatmak istediğinizde::

	$this->load->library('form_validation'); 

Bir kere başlattığınızda, kullanım kılavuzunda ilgili sayfadaki gösterilen sınıfı kullanabilirsiniz.

Ayrıca, birden fazla kütüphaneyi aynı anda yüklemek isterseniz load fonksiyonuna dizi gönderebilirsiniz.

::

	$this->load->library(array('email', 'table'));

Kendi Kütüphanenizi Oluşturmak
==============================

Lütfen Kullanım Kılavuzundaki :doc:`Kendi Kütüphanenizi Oluşturmak<creating_libraries>` kısmındaki tartışmayı okuyun.
