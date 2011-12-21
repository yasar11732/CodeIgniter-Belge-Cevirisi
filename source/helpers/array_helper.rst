###########
Dizi Helper
###########

Dizi helper dizilerle çalışırken işinizi kolaylaştıracak fonksiyonlar içermektedir.

.. contents:: Sayfa İçeriği

Helper Yüklemek
===============

Helper aşağıdaki kod ile yüklenir.:

::

	$this->load->helper('array');

Kullanılabilir fonksiyonlar:

element()
=========

.. php:method:: element($item, $array, $default = FALSE)

	:param string 	$item: Diziden çekilmesi istenilen indeks.
	:param array 	$array: Girdi dizisi.
	:param boolean	$default: Eğer dizi geçerli değilse geri dönecek değer.
	:returns: Hata durumunda ya da indeks dizide bulanmazsa FALSE döner.


Bu fonksiyon Dizi içinden istediğiniz anahtara karşılık gelen değeri almanızı sağlar. Fonksiyon anahtara karşılık gelen bir değer olup olmadığını kontrol eder, eğer tanımlı bir değer var ise bu değeri geri döndürür. Eğer değer tanımlanmamışsa FALSE döndürür. Değer tanımlanmadığı durumlarda FALSE dönmesini istemiyorsanız, geri dönmesini istediğiniz değeri 3. parametre olarak fonksiyona gönderebilirsiniz. Örnek

::

	$array = array(
		'color'	=> 'red',
		'shape'	=> 'round',
		'size'	=> ''
	);

	echo element('color', $array); // returns "red" 
	echo element('size', $array, NULL); // returns NULL 

elements()
==========

Bir dizi içinde birden fazla maddenin karşılığının bulunmasına izin verir. Fonksiyon, her bir indisin dizin içinde varlığını test eder. Eğer indeks yok ise FALSE ya da üüçncü parametrede ne tanımladıysanız o değeri geri döndürür.

.. php:method:: elements($items, $array, $default = FALSE)

	:param string 	$item: Diziden çekilmesi istenilen indeks.
	:param array 	$array: Girdi dizisi.
	:param boolean	$default: Eğer dizi geçerli değilse geri dönecek değer.
	:returns: Hata durumunda ya da indeks dizide bulanmazsa FALSE döner.

Örnek

::

	$array = array(
		'color' => 'red',  
		'shape' => 'round',     
		'radius' => '10',     
		'diameter' => '20'
	);

	$my_shape = elements(array('color', 'shape', 'height'), $array);

Yukarıdaki dizi şöyle geri döner

::

	array(
		'color' => 'red',     
		'shape' => 'round',     
		'height' => FALSE
	);

Üçüncü parametreyi istedğiniz gibi bir değere ayarlayabilirsiniz

::

	 $my_shape = elements(array('color', 'shape', 'height'), $array, NULL);

Yukarıdaki dizi şöyle geri döner

::

	array(     
		'color' 	=> 'red',     
		'shape' 	=> 'round',     
		'height'	=> NULL
	);

Bu kullanım $_POST dizisini model dosyasına gönderirken çok kullanışlıdır. Tabloya eklemek için ilaveten POST bilgisi hazırlamanızı önler.

::

	$this->load->model('post_model');
	$this->post_model->update(
		elements(array('id', 'title', 'content'), $_POST)
	);

Bu örnekte sadece id, title ve content alanları update edilecektir.

random_element()
================

Dizi içerisinden rasgele bir değer çeker ve geri döndürür:

.. php:method:: random_element($array)

	:param array 	$array: Girdi dizisi.
	:returns: String - Diziden rasgele çekilen bir değer.

::

	$quotes = array(
            "İstikbal göklerdedir. - M.Kemal ATATÜRK",
            "Adalet evrenin ruhudur. - Ömer HAYYAM",
            "Bir insanda kibir, hırs ve şehvet söz söylerken soğan kokar. - Mevlana",
            "Doğru düşündüğüne inanan yanlış fikirlerle savaşmak zorunda kalır. - Mehmet KAPLAN",
            ""Filozoflar dünyayı yalnızca çeşitli biçimlerde yorumlamışlardır; oysa sorun onu değiştirmektir. - Karl MARX",
            "Kulağım halkta,gözüm toplumda. - Rıfat ILGAZ"
	);

	echo random_element($quotes);

