##############
Tablo Verileri
##############

Bu fonksiyonlar tablo bilgilerini almanıza izin verirler.

$this->db->list_tables();
==========================

Bu fonksiyon o an bağlı olduğunuz veri tabanında bulunan tüm tabloların
isimlerini içeren bir dizi döndürür. Örneğin::

	$tablolar = $this->db->list_tables();
	
	foreach ($tablolar as $tablo)
	{
		echo $tablo;
	}

$this->db->table_exists();
===========================

Bazen özellikle bir tablonun üzerinde işlem yapmadan önce, o tablonun veri 
tabanında olup olmadığını bilmek size yardımcı olur. Bu fonkisyon boole 
TRUE/FALSE döndürür. Örnek kullanımı::

	if ($this->db->table_exists('tablo_adı'))
	{
		// yazdığınız kodlar...
	}

.. admonition:: Not
    :class: note

    *tablo_adı* değerini aradığınız tablonun adıyla değiştirin.
