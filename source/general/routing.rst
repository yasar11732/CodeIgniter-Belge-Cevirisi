###########
URI Yönlendirme
###########

Genellikle URL adresi ile onun controller sınıf/metodu arasında birebir ilişki mevcuttur. URI URI parçaları normalde şu deseni takip eder::

	example.com/class/function/id/

Bununla birlikte, bazı durumlarda, bu ilişkiyi yeniden düzenlemek isteyebilirsiniz. Yani, bir tane ilgili URL yerine farklı bir sınıf/fonksiyon çağrılabilir.

Örneğin, diyelimki URL'lerin şu kalıpta gelmesini istiyorsunuz:

example.com/product/1/
example.com/product/2/
example.com/product/3/
example.com/product/4/

Normalde URL'nin ikinci parçası fonksiyon adına rezerve edilmiştir, ama bu yukarıdaki örnekte onun yerine ürün ID değeri vardır. Bunun çaresine bakmak için, CodeIgniter URI işlemeyi yeniden düzenlemenize izin verir.

Kendi yönlendirme kurallarınızı ayarlamak
==============================

Yönlendirme kurallarınızı application/config/routes.php dosyasında tanımlayabilirsiniz. BU dosya içinde çağırdığınız $route dizini, kendi yönlendirme kriterinizi tanımlamanıza izin verir. Yönlendirme tanımlanırken wildcards ya da Regular Expressions kullanılabilir.

Wildcards
=========

Tipik bir wildcard yönlendirmesi şuna benzeyecektir::

	$route['product/:num'] = "catalog/product_lookup";

Yönlendirmede dizi anahtarı, varılacak hedef bilgisine göre yeniden yönlendirilecek değere eşleştirilecek URI değerini içerir. Yukarıdaki örnekte, eğer harfiyen "product" kelimesi URL'nin ilk parçasında ve bir sayı da ikinci parçada bulunursa, bu değerler yerine "catalog" sınıfı ve "product_lookup" metodu kullanılacaktır.

Kelimeye harfiyen ya da iki farklı wildcard kullanarak eşleştirme yapabilirsiniz:

**(:num)** parça sadece sayı ise eşleştirir.
 **(:any)** parça sadece karakter ise eşleştirir.

.. not:: Yönelendirme sadece tanımlıysa çalışır. Yükek öncelikteki yönlendirmeler düşüklere göre her zaman önce çalışır.

Örnekler
========

Burada bir kaç tane yönlendirme örneği gösterelim::

	$route['journals'] = "blogs";

URl içerisinde "journals" kelimesi ilk parçada geçtiğinde "blogs" sınıfına yönlendirilir.

::

	$route['blog/joe'] = "blogs/users/34";

URl içerisindeki parçada blog/joe parçası geçtiğinde "blogs" sınıfına "users" metoduna yönlendirilir. ID değeri "34" olarak ayarlanır.

::

	$route['product/(:any)'] = "catalog/product_lookup";

URL'nin ilk parçası "product" ve ikinci parçası herhangi bir şey olduğunda, hemen "catalog" sınıfı, "product_lookup" metoduna yönlendirilir.

::

	$route['product/(:num)'] = "catalog/product_lookup_by_id/$1";

URL'nin ilk parçası "product" ve ikinci parçası herhangi bir şey olduğunda, hemen "catalog" sınıfı, "product_lookup_by_id" metoduna ne kadar parametre varsa gönderilir.

.. Önemli:: Önceye ya da sonraya ters bölü işareti kullanmayın.

Düzenli İfadeler (Regular Expressions)
===================

Eğer isterseniz, yönlendirme kullarınızda düzenli ifadeler kullanabilirsiniz. Geçerli olan düzenli ifadelerin kullanımı geri-referanslı olmaları durumunda izin verilir.

.. not:: Eğer geri-referanslı kullanacaksanız, çift ters bölü işareti yerine dolar işaretini tercih edin.

Tipik bir RegEx yönlendirmesi şöyledir::

	$route['products/([a-z]+)/(\d+)'] = "$1/id_$2";

Yukarıdaki örnekte, products/shirts/123 gibi olan bir URI, shirts controller sınıfı ve id_123 fonksiyonunu çağıracaktır.

Ayrıca yönlendirme için wildcards ve düzenli ifadeleri birlikte de kullanabilirsiniz.

Rezerve Yönlendirmeler
===============

İki tane rezerve yönlendirme vardır::

	$route['default_controller'] = 'welcome';

Bu yönlendirme URI içinde herhangi bir bilgi yoksa, insanlar kök URL adresini çağırdıklarında yüklenecek controller sınıfını işaret eder. Yukarıdaki örnekte, "welcome" sınıfı yüklenir. Varsayılan yönlendirme için kendi 404 sayfanızın görünmesini sağlayabilirsiniz.

::

	$route['404_override'] = '';

Bu yönlendirmede controller sınıfı bulunamadığında yüklenecek olan controller gösterilmektedir. Varsayılan 404 hata sayfasının üzerine yazılır. Eğer varsayılan  application/errors/error_404.php dosyası yüklenecekse, show_404() fonksiyonu etkilenmeyecektir.

.. önemli:: Rezerve yönlendirmeler herhangi bir wildcards ya da düzenli ifade yönlendirmelerinden önce olmalıdır.
