###################################
Karşılaştırma (Benchmarking) Sınıfı
###################################

CodeIgniter belirlenmiş iki nokta arasındaki harcanan zamanı hesaplamak için her zaman aktif olan bir karşılaştırma (Benchmark) sınıfına sahiptir.

.. admonition:: Not
    :class: note

    Bu sınıf sistem tarafından otomatik olarka yüklenir, elle yüklenmesine gerek yoktur.

Ek olarak: sınıf, framework başladığı anda aktifleştirilir ve çıktı(output) sınıfı tarafından view(görüntü) tarayıcıya gönderilmeden hemen önce sonlandırılır, sistemin çalışma zamanı hakkında kesin sonuçlar üretir.

.. contents:: Başlıklar

Karşılaştırma sınıfının kullanılması
====================================

view, model ve controller Karşılaştırma sınıfı uygulamanızdaki :doc:`controllers </general/controllers>`, :doc:`views </general/views>`, ya da :doc:`models </general/models>` tarafından çağrılabilir. Kullanım örneği :

#. Başlangıç noktası belirle
#. Bitiş noktası belirle
#. "elapsed_time" fonskyionu ile sonucu al

Örnek kodlar ile örnek verelim::

	$this->benchmark->mark('code_start');

	// Burda bir miktar kod çalışıyor

	$this->benchmark->mark('code_end');

	echo $this->benchmark->elapsed_time('code_start', 'code_end');

.. admonition:: Not
    :class: note

    "code_start" ve "code_end" kelimeleri yerine istediğiniz başka kelimelerde kullanabilirsiniz, ayrıca 2 den fazla nokta tanımlayıp aralarındaki zaman farkını da hesaplayabilirsiniz. Aşağıda bir örnek mevcut ::

		$this->benchmark->mark('dog');

		//  Burada bir miktar kod çalışıyor

		$this->benchmark->mark('cat');

		//  Burada da bir miktar kod çalışıyor

		$this->benchmark->mark('bird');

		echo $this->benchmark->elapsed_time('dog', 'cat');
		echo $this->benchmark->elapsed_time('cat', 'bird');
		echo $this->benchmark->elapsed_time('dog', 'bird');


Uygulama profili ile kullanılması
=================================

Karşılaştırma sonuçlarınızın :doc:`Profiler </general/profiling>` ile kullanılabilir olmasını istiyorsanız mark fonskiyonunun alacağı parametreler belirli bir düzen içerisinde olmalıdır. Parametre düzeni şöyledir : parametreler çiftler halinde yazılır _start ve _end ile biterler. Örnek kullanım ::

	$this->benchmark->mark('my_mark_start');

	// Burada da bir miktar kod çalışıyor...

	$this->benchmark->mark('my_mark_end'); 

	$this->benchmark->mark('another_mark_start');

	// Burada da bir miktar kod çalışıyor...

	$this->benchmark->mark('another_mark_end');

Lütfen daha fazla uygulama için :doc:`Profiler page </general/profiling>` sayfasına göz atın.
	
Toplam çalışma süresinin gösterilmesi
=====================================

Çalışan kodun en baştan en sonra kadar ne kadar zamanda çalıştığını göstermek isterseniz view dosyanızın herhangi bir yerine aşağıdaki kodu yazabilirsiniz::

	<?php echo $this->benchmark->elapsed_time();?>

elapsed_time() fonksiyonunu daha önceden hatırlıyoruz, iki nokta arasında ki geçen zamanı elde etmek için bu fonksiyonu Kullanımıştık, şimdi farklı olarak parametre **kullanmadan** çağırdık. Fonksiyon çağrılırken parametre belirtilmezse karşılaştırma sınıfı kodun çalışmaya başlaması ve view dosyasının tarayıcıya gönderilmesi arasındaki geçen zamanı hesaplayıp döndürecektir, bu sebepten dolayı kodun nerede çağtırıldığının bir önemi yoktur, nerde çağrılırsa çağrılsın aynı sonucu döndürecektir.	

Toplam zamanı göstermenin alternatif bir yolu da sahte değişken kullanımıdır. Php kodu Kullanımak istemediğiniz zamanlar size yardımcı olabilir. Örnek kullanım ::

	{elapsed_time}

.. admonition:: Not
    :class: note

    Kontrol dosyanız içerisinden ölçümler yapmak istiyorsanız, kendi başlangıç ve bitiş noktalarınızı tanımlamalısınız.

Bellek kullanımının gösterilmesi
================================

Eğer php kurulumunuz –enable-memory-limit konfigurasyonu ile derlendiyse aşşağıdaki kodu kullanarak, çalışan scriptin harcadığı bellek miktarını elde edebilirsiniz ::

	<?php echo $this->benchmark->memory_usage();?>

.. admonition:: Not
    :class: note
    
    Bu fonksiyon sadece view dosyası içierisinden çağrılabilir. İçinde çalıştırıldığı scriptin o anki bütün bellek kullanımını verir.

Toplam bellek kullanımını göstermenin alternatif bir yolu da sahte değişken tanımlamaktır. Php kodu Kullanımak istemediğiniz zamanlar size yardımcı olabilir. Örnek kullanım ::

	{memory_usage}

