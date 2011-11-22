##################
Genel Fonksiyonlar
##################

CodeIgniter kendi kullanımı için bir kaç tane fonksiyonu genel olarak tanımlar, bu fonksiyonlar her hangi bir noktada sizin de kullanımınıza açıktır. Herhangibi bir kütüphane ya da helper ile yüklemenize gerek yoktur.

is_php('version_number')
========================

is_php() kullanılan PHP versiyonunun, Versiyon_number ile tanımlı değerden büyüklüğünü kontrol eder.

::

	if (is_php('5.3.0'))
	{
	    $str = quoted_printable_encode($str);
	}

Eğer kullanılan versiyon, sorulan versiyon numarasına eşit ya da büyükse geri dönüşü TRUE olur. Geri dönüş FALSE ise, kullanılan PHP versiyonu sorulan versiyon numarasından daha küçük demektir.

is_really_writable('path/to/file')
==================================

is_writable() fonksiyonu Windows sunucu üzerinde PHP kullanıldığı zaman, dosya yazılamayan dizinlerde dahi TRUE olarak dönebilir. Oysa bu fonksiyon çalıştığında öncelikle gerçek bir dosya yazmaya çalışarak sonuca karar verir. Genellikle güvenilir sonuçlar elde edilemeyen platformlarda kullanılması önerilir.

::

	if (is_really_writable('file.txt'))
	{
	    echo "I could write to this if I wanted to";
	}
	else
	{
	    echo "File is not writable";
	}

config_item('item_key')
=======================

Yapılandırma bilgilerine erişim :doc:`Config library <../libraries/config>` ile kullanıması tercih edilse de, config_item() fonksiyonu da tekil değerler için kullanılabilir. Daha fazla bilgi için Yapılandırma Kütüphanesi bölümüne bakınız.

show_error('message'), show_404('page'), log_message('level', 'message')
========================================================================

Her biri için :doc:`Hata İşleme <errors>` sayfasına bakınız.

set_status_header(code, 'text');
================================

Sunucu durum başlığını elinizle düzeltmenize izin verir. Örneğin::

	set_status_header(401);
	// Sets the header as:  Unauthorized

Tüm başlıklara ait liste için `burayı <http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html>`_ okuyunuz.	
	

remove_invisible_characters($str)
=================================

Bu fonksiyon ascii karakterler ile boş (null) karakter girişini önler (Java\\0script gibi).

html_escape($mixed)
===================

Bu fonksiyon, htmlspecialchars() fonsiyonunun kısayoludur. String ve dizi değişkenlerini kabul eder. Cross Site Scripting (XSS) yapılmasını kolayca önler.
