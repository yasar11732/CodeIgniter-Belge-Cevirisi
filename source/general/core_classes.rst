############################
Çekirdek Sistem Sınıfı Oluşturmak
############################

CodeIgniter herzaman çekirdek çatının bir parçası olarak başlangıçta otomatik bazı temel sınıflar yükler. Bununla birlikte, bazı çekirdek sistem sınıflarının kendi versiyonlarınızla yer değiştirmesi ya da çekirdek versiyonlarının genişletilmesi de mümkündür.

**Bir çok kullanıcının asla ihtiyaç duymayacağı bu opsiyon, CodeIgniter çekirdeğinde belirgin bir değişiklik izi bırakmak isteyenlere olanak sağlar.**

.. not:: Çekirdek sistem sınıflarını karıştırma bir çok olumsuzluğu getireceği için işe başlamadan önce ne yaptığınızı bildiğinizden emin olun.

Sistem Sınıfları Listesi
=================

Aşağıdaki liste CodeIgniter'ın her başladığında çalıştırdığı çekirdek sistem dosyaları listesidir:

-  Benchmark
-  Config
-  Controller
-  Exceptions
-  Hooks
-  Input
-  Language
-  Loader
-  Log
-  Output
-  Router
-  URI
-  Utf8

Çekirdek Sınıfları Değiştirmek
======================

Kendinize ait bir sistem sınıfını mevcut sınıfla değiştirmek için, dosyanın kendi versiyonunu application/libraries dizini altına yerleştirin::

	application/core/some-class.php

Eğer bu dizin mevcut değilse oluşturun.

Yukarıdaki listede bulunanlardan aynı isime sahip olanlar, normalde kullanılanlar yerine kullanılacaktır.

Lütfen şunu unutmayın, kendi sınıflarınız CI öneki kullanmak zorundadır. Örneğin, eğer dosya adınız Input.php ise, sınıf adı şöyle olmalıdır::

	class CI_Input {

	}

Çekirdek Sınıfı Genişletmek
====================

Eğer tüm ihtiyacınız mevcut kütüphaneye bazı işlevsellikler kazandırmak -belki bir ya da iki fonskyion eklemek-, sonra da kullandığınız mevcut kütüphaneyi sizin kütüphanenizle yerdeğiştirmek ve onu durdurmak ise, bu durumda sınıfı genişletmek daha uygun olur. Sınıf genişletmek neredeyse, mevcut sınıf ile yer değiştirmek gibidir, şu farkla:

-  Ana sınıf bildirimi mutlaka yapılmalıdır.
-  Sizin sınıf ve dosya adınız mutlaka MY\_ öneki ile başlamalıdır (Bu değer değiştirilebilir. Aşağı bakın.).

Örneğin, mevcut Input sınıfı için, application/libraries/MY_Input.php dosyasını oluşturmalı ve kendi sınıfınızda bildirim yapmalısınız:

	class MY_Input extends CI_Input {

	}

Not: Eğer sınıfınızda constructor kullanmaya ihtiyacınız varsa, ana constructor'ü de genişletmelisiniz::

	class MY_Input extends CI_Input {

	    function __construct()
	    {
	        parent::__construct();
	    }
	}

**İpucu**:  Sınıfınız bulunan ve ana sınıfın adı ile aynı ada sahip olan herhangi bir fonksiyon varsa, bu fonksiyon mevcut olanın yerine kullanılır ("üzerine yazdırma metodu" olarak da bilinir). Bu özellik CodeIgniter çekirdeğini büyük olçüde değiştirmenize olanak tanır.

Eğer controller çekirdek sınıfını genişlettiyseniz, uygulamanızda kullandığınız controller'ın constructor sınıfında bu yeni sınıfın genişletilmesi ile oluşturduğunuzu kontrol ediniz.

::

	class Welcome extends MY_Controller {

	    function __construct()
	    {
	        parent::__construct();
	    }

	    function index()
	    {
	        $this->load->view('welcome_message');
	    }
	}

Kendi Önekinizi Ayarlamak
-----------------------

Kendi alt-sınıfınızda kullanacağını önekinizi ayarlamak için, application/config/config.php dosyasını açın ve şunu arayın::

	$config['subclass_prefix'] = 'MY_';

Lütfen unutmayın, CodeIgniter'ın bütün mevcut kütüphaneleri CI\_ önekini kullanır, bu nedenle siz KULLANMAYIN.
