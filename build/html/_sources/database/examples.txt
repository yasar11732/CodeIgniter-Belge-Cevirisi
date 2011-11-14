#########################################
Veri Tabanı Hızlı Başlangıç: Örnek Kodlar
#########################################

Bulunduğunuz sayfa veri tabanı sınıfının nasıl kullanılacağını gösteren 
örnekler içerir. Daha detaylı bilgi için lütfen fonksiyonların kendi 
sayfalarını okuyun.

Veri Tabanı Sınıfının Başlatılması
==================================

Aşağıda komut :doc:`konfigürasyon <configuration>` dosyanızdaki ayarlara
bağlı olarak veri tabanı sınıfını yükler ve çalıştırır::

	$this->load->database();

Sınıf bir kere yüklendiğinde aşağıda anlatıldığı gibi kullanmaya hazırdır.

Not: Eğer tüm sayfalarınız veri tabanı bağlantısı gerektiriyorsa, veri 
tabanına otomatik olarak bağlanabilirsiniz. Detaylar için :doc:`bağlantı <connecting>`
sayfasına göz atınız.

Çoklu Sonuç Döndüren Standart Sorgu (Nesne Versiyonu)
=====================================================

::

	$query = $this->db->query('SELECT ad, unvan, eposta FROM tablom');
	
	foreach ($query->result() as $row)
	{
		echo $row->unvan;
		echo $row->ad;
		echo $row->eposta;
	}
	
	echo 'Toplam Sonuç: ' . $query->num_rows();

Yukarıdaki result() fonksiyonu bir **nesne** dizisi döndürür. Örneğin:
$row->unvan

Çoklu Sonuç Döndüren Standart Sorgu (Dizi Versiyonu)
====================================================

::

	$query = $this->db->query('SELECT ad, unvan, eposta FROM tablom');
	
	foreach ($query->result_array() as $row)
	{
		echo $row['ad'];
		echo $row['unvan'];
		echo $row['eposta'];
	}

Yukarıdaki result_array() fonksiyonu standart dizi indekslerinden oluşan
bir dizi döndürür. Örneğin: $row['unvan']

Sonuçların Kontrol Edilmesi
===========================

Yapılan sorgular her zaman bir sonuç döndürmek zorunda **değildir**. Bunu
kontrol etmek için num_rows() fonksiyonunu kullanabilirsiniz::

	$query = $this->db->query("SQL SORGUNUZ");
	if ($query->num_rows() > 0)
	{
		foreach ($query->result() as $row)
		{
			echo $row->unvan;
			echo $row->ad;
			echo $row->metin;
		}
	}

Tek Sonuç Döndüren Standart Sorgu
=================================

::

	$query = $this->db->query('SELECT ad FROM tablom LIMIT 1'); 
	$row = $query->row();
	echo $row->ad;

Yukarıdaki row() fonksiyonu bir **nesne** döndürür. Örneğin: $row->name

Tek Sonuç Döndüren Standart Sorgu (Dizi Versiyonu)
==================================================

::

	$query = $this->db->query('SELECT ad FROM tablom LIMIT 1');
	$row = $query->row_array();
	echo $row['ad'];

Yukarıdaki row_array() fonksiyonu bir **dizi** döndürür. Örneğin:
$row['ad']

Standart Kayıt Ekleme
=====================

::

	$sql = "INSERT INTO tablom (unvan, ad) VALUES (".$this->db->escape($unvan).", ".$this->db->escape($ad).")";
	$this->db->query($sql);
	echo $this->db->affected_rows();

Aktif Kayıt Yöntemi İle Sorgu
=============================

:doc:`Aktif Kayıt Modeli <active_record>` veriyi daha basit bir şekilde
almanıza olanak sağlar.::

	$query = $this->db->get('tablo_adi');
	
	foreach ($query->result() as $row)
	{
		echo $row->title;
	}

Yukarıdaki get() fonksiyonu sorgulanan tablodaki tüm sonuçları getirir. 
:doc:`Aktif Kayıt <active_record>` sınıfı bir veri ile çalışmak için
gereken tüm fonksiyonları içerir.

Aktif Kayıt Yöntemi İle Kayıt Ekleme
====================================

::

	$veri = array(
		'unvan' => $unvan,
		'ad' => $ad,
		'tarih' => $tarih
	);
	
	$this->db->insert('tablom', $veri);  // Üretilen sorgu: INSERT INTO tablom (unvan, ad, tarih) VALUES ('{$unvan}', '{$ad}', '{$tarih}')

