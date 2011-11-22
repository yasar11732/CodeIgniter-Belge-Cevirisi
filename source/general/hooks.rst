###################################
Hook - Çatı Çekirdeğini Genişletmek
###################################

CodeIgniter'ın Hook özelliği, çekirdek dosyaları ele geçirmeden çatıya girmenin ve içeride çalışmanın yolunu sağlar. CodeIgniter çalıştığında :doc:`Uygulama Akışında <../overview/appflow>` tanımlı yolu uygular. Bununla birlikte, uygulamanın çalışmasıyla belirli aşamalarında uygulamaya müdahale etmek isteyebilirsiniz. Örneğin, controller dosyasının yüklenmesinden önce ya da sonra bir script çalıştırmak, ya da başka bir yerde bulunan bir scriptinizi tetiklemek isteyebilirsiniz.

Hook'u Etkinleştirmek
=====================

Hook özelliğini tüm uygulamada etkinleştirip, etkisizleştirmek için application/config/config.php dosyasında şu madde ayarlanmalıdır::

	$config['enable_hooks'] = TRUE;

Bir Hook Tanımlamak
===================

Hooks application/config/hooks.php dosyasında tanımlanır. Her Hook dizide bir prototip olarak tanımlanır::

	$hook['pre_controller'] = array(
	                                'class'    => 'MyClass',
	                                'function' => 'Myfunction',
	                                'filename' => 'Myclass.php',
	                                'filepath' => 'hooks',
	                                'params'   => array('beer', 'wine', 'snacks')
	                                );

**Notlar:**
Dizi indeksi, kullanmak istediğiniz hook adı ile ilişkilendirilir. Yukarıdaki örnekte, hook noktası pre_controllerdır. Hook noktalarının listesi aşağıdadır. Takip eden maddeleri hook dizisinde ilişkili olarak tanımlanmalıdır:

-  **class** Başlatmak istediğiniz sınıfın adıdır. eğer sınıf yerine fonksiyon başlatmak istiyorsanız, bu maddeyi boş bırakın.
-  **function** Çağırmak istediğiniz fonksiyonun adı.
-  **filename** Sınıf/fonksiyonun bulunduğu dosya adı.
-  **filepath** criptinizin bulunduğu dizinin adıdır. Not: Scriptiniz application dizini İÇİNDE bulunmalı, filepath bağıl olarak bu dizini işaret etmelidir. Örneğin, eğer scriptiniz application/hooks dizini altındaysa, filepath değeri için hooks kullanmalısınız. Eğer scriptiniz application/hooks/utilities dizini altındaysa, filepath için hooks/utilities değerini kullanmalısınız. Sona ters bölü eklemeyin.
-  **params** Scriptinize göndermek istediğiniz herhangi bir parametre. Bu madde opsiyoneldir.

Aynı Hook'a Çoklu Çağırmak
==========================

Eğer birden fazla scripti aynı hook noktasında kullanmak isterseniz, çok-boyutlu dizi tanımlamanız yeterlidir, mesela::

	$hook['pre_controller'][] = array(
	                                'class'    => 'MyClass',
	                                'function' => 'Myfunction',
	                                'filename' => 'Myclass.php',
	                                'filepath' => 'hooks',
	                                'params'   => array('beer', 'wine', 'snacks')
	                                );

	$hook['pre_controller'][] = array(
	                                'class'    => 'MyOtherClass',
	                                'function' => 'MyOtherfunction',
	                                'filename' => 'Myotherclass.php',
	                                'filepath' => 'hooks',
	                                'params'   => array('red', 'yellow', 'blue')
	                                );

Her dizi indeksinden sonra köşeli paranteze dikkat edin::

	$hook['pre_controller'][]

Bu, sizin çoklu scriptlerli aynı hook içinde kullanmanıza izin verir. Dizinlerinizi çalışma önceliğine göre sıralayın.

Hook Noktaları
==============

Mevcut hook noktaları listesi aşağıdadır.

-  **pre_system**
   Sistem çalışmadan çok önce çağırılma. Sadece benchmark ve hook sınıfları yüklenmiştir bu noktada. Yönlendirme ya da diğer işlemler gerçekleşmez.
-  **pre_controller**
   Herhangi bir controller çalışmadan önce çağrılma. Bütün temel sınıflar, yönlendirme ve güvenlik kontrolleri yapılmıştır.
-  **post_controller_constructor**
   Controller yüklendikten hemen sonra, metodların başlatılmasından önce derhal çağrılma.
-  **post_controller**
   Controller dosyası tamamen çalıştırıldıktan sonra hemen çağrılma.
-  **display_override**
	_display() fonksiyonunun üzerine yazmak, sistem çalışması bittikten sonra web tarayıcıya finalize edilerek sayfa göndermekte kullanılır. Bu opsiyon, kendi ekran gösterin metodunuzu kullanmaya izin verir. Bu opsiyonu kullanırken $this->CI =& get_instance() ile CI superobject'e referans vermeyi ve finalize edilmiş bilgileri $this->CI->output->get_output() ile çağırmayı unutmayın.
-  **cache_override**
	Output sınıfnıdaki _display_cache() fonksiyonu yerine kendi fonksiyonlarınızı çağırma olanağı. Bu opsiyon, kendi bellek gösterim mekanizmanızı kullanmaya izin verir.
-  **post_system**
	Sistem çalıştırılması bittikten sonra tarayıcıya gönderilen finalize edilmiş sayfanın ardından çağrılma.

