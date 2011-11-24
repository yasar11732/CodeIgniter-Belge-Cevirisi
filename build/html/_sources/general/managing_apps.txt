######################
Uygulamaların Yönetimi
######################

CodeIngiter system/application/ dizinine kurularak sadece bir uygulamayı çalıştıdığınız varsayılır. Bununla birlikte, bir tane CodeIgniter kurulumu yaparak, application yerini değiştirerek ya da ismini değiştirerek çoklu uygulamayı çalıştırmanız da mümkündür.

Application Dizininin Adını Değiştirmek
=======================================

Eğer application dizininin adını değiştirmek istiyorsanız, bunu ana dizindeki index.php dosyasını açıp, $application_folder değişkeninin adını değiştirerek yapabilirsiniz::

	$application_folder = "application";

Application Dizininin Yerini Değiştirmek
========================================

Application dizininin yerini system dizini altından farklı bir yere almak da mümkündür. Bundan sonra yapacağınız, index.php dosyasındaki $application_folder değişkenine **tüm sunucu yolunu** tanımlamaktır.

::

	$application_folder = "/Path/to/your/application";

Çoklu Uygulamayı bir CodeIgniter Kurulumu ile Çalıştırmak
=========================================================

Eğer bir CodeIgniter kurulumunu bir kaç farklı uygulama arasında basitçe paylaştırmak istiyorsanız, bütün uygulamalarınızı, herbiri farklı isimdeki dizinler ile birlikte application dizini içine yerleştiriniz.

Örneğin, diyelim ki "foo" ve "bar" isminde iki farklı uygulama oluşturmak istiyorsunuz. Uygulama dizininizin yapısı şuna benzeyecektir::

	applications/foo/
	applications/foo/config/
	applications/foo/controllers/
	applications/foo/errors/
	applications/foo/libraries/
	applications/foo/models/
	applications/foo/views/
	applications/bar/
	applications/bar/config/
	applications/bar/controllers/
	applications/bar/errors/
	applications/bar/libraries/
	applications/bar/models/
	applications/bar/views/

Kullanacağınız uygulamayı belirtmek için ana index.php dosyasını açıp, $application_folder değişkenini ayarlamalısınız. Örneğin, "foo" uygulamasını seçmek için şunu yapmalısınız::

	$application_folder = "applications/foo";

.. admonition:: NOT
    :class: note

    Her uygulamanın kendisine ait, kendisini çağıran bir index.php dosyası olmalıdır. Index.php dosyasının adı isteğinize göre herşey olabilir.
