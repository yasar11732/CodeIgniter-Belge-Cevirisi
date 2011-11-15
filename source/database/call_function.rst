########################
Özel Fonksiyon Çağrıları
########################

$this->db->call_function();
============================

Bu fonksiyon Codeigniter ile beraber gelmeyen PHP veri tabanı fonksiyonlarını
çalıştığınız platformdan bağımsız olarak çağırmanıza olanak tanır. Örneğin, 
Codeigniter tarafından **desteklenmeyen** mysql_get_client_info() fonksiyonunu 
çağırmak istiyorsunuz. Bunu şu şekilde yapabilirisiniz::

	$this->db->call_function('get_client_info');

Fonksiyonun adını başında mysql\_ **olmadan** ilk parametre olarak yazmalısınız.
mysql\_ ön eki hangi o an kullanılan veri tabanı tipine bağlı olarak otomatik
olarak eklenecektir. Bu durum size aynı fonksiyonu farklı veri tabanı ortamlarında
çalıştırma izini verir. Şu durum açıktır ki bir fonksiyon farklı ortamlarda tamamen aynı
şekilde çağrılamaz. Dolayısıyla fonksiyonun ortamlar arası uyumluluğuna bağlı olarak
bazı sınırlamalar olacaktır.

Çağırdığınız fonksiyonun ihtiyaç duyduğu parametreler, fonksiyon adından sonra eklenir.

::

	$this->db->call_function('çağrılan_fonksiyon', $param1, $param2, etc..);

Uygulamada sıklıkla veri tabanı bağlantı ID'sine, veri tabanı sonuç ID'sine ya da her
ikisine ihtiyaç duyabilirsiniz. Bağlantı ID'sine aşağıdaki şekilde ulaşılabilir::

	$this->db->conn_id;

Sonuç ID'sine, yapılan sorgunun sonuç nesnesinden şu şekilde erişilebilir.::

	$sorgu = $this->db->query("SQL SORGUNUZ");
	
	$sorgu->result_id;