##################
Alan Meta Verileri
##################

$this->db->list_fields()
=========================

Alan adlarını içeren bir dizi döndürür. Bu sorgu iki şekilde çağırılabilir:

1. Tablo adını parametre olarak verebilir ve $this->db nesnesinden çağrabilirsiniz::
object::

	$alanlar = $this->db->list_fields('tablo_adı');
	
	foreach ($alanlar as $alan_adi)
	{
		echo $alan_adi;
	}

2. Sorgu sonucu nesnesinden bu fonksiyonu çağırarak, yapılan sorgu ile ilişkili 
alan adlarını bir araya getirebilirsiniz.::

	$sorgu = $this->db->query('SELECT * FROM bir_tablo');
	
	foreach ($sorgu->list_fields() as $alan_adi)
	{
		echo $alan_adi;
	}

$this->db->field_exists()
==========================

Bazen özellikle bir alanın üzerinde işlem yapmadan önce, o alanın var 
olup olmadığını bilmek size yardımcı olur. Bu fonkisyon boole TRUE/FALSE 
döndürür. Örnek kullanımı::

	if ($this->db->field_exists('alan_adı', 'tablo_adı'))
	{
		// yazılan kodlar...
	}

.. not:: *alan_adı* parametresini aradığınız kolonun adıyla ve *tablo_adı*
	parametresini üzerinde arama yapmak istediğiniz tablo adıyla değiştirin.

$this->db->field_data()
========================

Alan bilgisi içeren bir nesne dizisi döndürür.

Bazen alan alan adlarını veya kolon tipi, maksimumum uzunluk, vb. meta verileri 
almak, size yardımcı olabilir.

.. not:: Bazı veri tabanları meta veri desteği sağlamayabilir.

Kullanım örneği::

	$alanlar = $this->db->field_data('tablo_adi');
	
	foreach ($alanlar as $alan)
	{
		echo $alan->name;
		echo $alan->type;
		echo $alan->max_length;
		echo $alan->primary_key;
	}

Zaten çalıştırmış olduğunuz bir sorgunuz varsa fonksiyona parametre olarak tablo 
adı vermek yerine sonuç nesnesini kullanabilirsiniz::

	$sorgu = $this->db->query("SQL SORGUNUZ");
	$alanlar = $sorgu->field_data();

Eğer veri tabanınız tarafından destekleniyorsa, bu fonksiyon aşağıdaki verileri 
döndürür:

-  name - kolon adı
-  max_length - kolonun saip olduğu maksimum uzunluk değeri
-  primary_key - eğer kolon birincil anahtar (primary key) ise 1 
-  type - kolonun tipi