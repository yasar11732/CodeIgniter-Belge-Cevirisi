############
Dizin Helper
############

Dizin yardımcısı dizinlerle çalışırken ihtiyaç duyacağınız bazı fonksiyonları barındırır.

.. contents:: Sayfa İçeriği

Helper Yüklemek
===============

Bu helper aşağıdaki şekilde yüklenilir

::

	$this->load->helper('directory');

Kullanılabilir fonksiyonlar:

directory_map()
===============

Bu fonksyion parametre olarak gönderilen dizini tarayarak içerdiği dosyaları bir dizi değişkeni şeklinde sunar.
	
.. php:method:: directory_map($source_dir[, $directory_depth = 0[, $hidden = FALSE]])

	:param string	$source_dir: kaynak dizin yolu
	:param integer	$directory_depth: iniliecek dizin derinliği (0 =
		hepsini getir, 1 = bulunduğu dizini getir, vs.)
	:param boolean	$hidden: görünmeyen dizileri de getir
	
Örnekler::

	$map = directory_map('./mydirectory/');

.. admonition:: Not
    :class: note

    Parametre olarak tam dizin yolu verilmezse aktif dizin index.php dosyanızın bulunduğu dizin olarak kabul edilecektir.


Yukarıdaki fonksiyon çağrımı ile alt dizin ve dosyalar da diziye dahil edilecektir. Sadece dizin içeriğini almak istiyorsaniz alt dizinleri almak istemiyorsanız ikinci parametreyi true olarak gönderebilirsiniz. Örnek::

	$map = directory_map('./mydirectory/', 1);

Ön tanımlı olarak gizli dizin ve dosyalar diziye dahil edilmeyecektir, gizli dizin ve dosyaları da almak isterseniz 3. parametreyi true (boolean) olarak gönderebilirsiniz::

	$map = directory_map('./mydirectory/', FALSE, TRUE);

arama sonucunda bulunan her dizin bir dizi olarak tanımlanacak ve bulunan her dosya 0 dan başlayarak yükselen sayısal bir değer alacaktır. Örnek olması açısından fonksiyondan dönen bir dizi aşağıda listelenmiştir::

	Array (    
		[libraries] => Array    
			(        
				[0] => benchmark.html        
				[1] => config.html        
				[database] => Array
					(              
						[0] => active_record.html              
						[1] => binds.html              
						[2] => configuration.html
						[3] => connecting.html              
						[4] => examples.html              
						[5] => fields.html              
						[6] => index.html
						[7] => queries.html
					)        
				[2] => email.html        
				[3] => file_uploading.html        
				[4] => image_lib.html        
				[5] => input.html        
				[6] => language.html        
				[7] => loader.html        
				[8] => pagination.html        
				[9] => uri.html
			)

