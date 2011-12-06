#################
HTML Tablo Sınıfı
#################

Tablo sınıfı array veya veritabanı sonuç setlerinden otomatik olarak HTML tablosu oluşturmanıza imkan sağlayacak işlevler sunar.

Sınıfı başlatma
===============

Codeigniter'daki birçok sınıf gibi HTML Tablo Sınıfı da controller içinde $this->load->library işlevi kullarak başlatılır::

	$this->load->library('table');

Bir kere yüklendikten sonra, Tablo kütüphanesi objesine şöyle erişilebilir: $this->table

Örnekler
========

Çok boyutlu bir array'den nasıl tablo oluşturacağınızı gösteren bir örnek. İlk array'in tablo başlığı olacağını unutmayın (yada aşağıdaki işlev referansında tarif edilen set_heading() işlevini kullarak kendi başlığınızı ayarlayabilirsiniz).

::

	$this->load->library('table');

	$data = array(
	             array('Name', 'Color', 'Size'),
	             array('Fred', 'Blue', 'Small'),
	             array('Mary', 'Red', 'Large'),
	             array('John', 'Green', 'Medium')	
	             );

	echo $this->table->generate($data);

Veritabanı sorgusu sonuçlarından oluşturulan tablo örneği. Tablo sınıfı veritabanı tablo isimlerine dayanarak, tablo başlıklarını otomatik olarak oluşturacak (yada aşağıdaki işlev referansında tarif edilen set_heading() işlevini kullarak kendi başlığınızı ayarlayabilirsiniz).

::

	$this->load->library('table');

	$query = $this->db->query("SELECT * FROM my_table");

	echo $this->table->generate($query);

Ayrık parametreler kullanarak nasıl tablo oluşturabileceğinizi gösteren bir örnek::

	$this->load->library('table');

	$this->table->set_heading('Name', 'Color', 'Size');

	$this->table->add_row('Fred', 'Blue', 'Small');
	$this->table->add_row('Mary', 'Red', 'Large');
	$this->table->add_row('John', 'Green', 'Medium');

	echo $this->table->generate();

Aynı örnek, yalnızca bireysel	 parametreler yerine array kullanılmış::

	$this->load->library('table');

	$this->table->set_heading(array('Name', 'Color', 'Size'));

	$this->table->add_row(array('Fred', 'Blue', 'Small'));
	$this->table->add_row(array('Mary', 'Red', 'Large'));
	$this->table->add_row(array('John', 'Green', 'Medium'));

	echo $this->table->generate();

Tablonuzun Görüntüsünü Değiştirme
=================================

Tablo sınıfı, tablo tasarımını belirtebileceğiniz bir tablo şablonu ayarlamanıza izin verir. Şablon prototipi şöyledir::

	$tmpl = array (
	                    'table_open'          => '<table border="0" cellpadding="4" cellspacing="0">',

	                    'heading_row_start'   => '<tr>',
	                    'heading_row_end'     => '</tr>',
	                    'heading_cell_start'  => '<th>',
	                    'heading_cell_end'    => '</th>',

	                    'row_start'           => '<tr>',
	                    'row_end'             => '</tr>',
	                    'cell_start'          => '<td>',
	                    'cell_end'            => '</td>',

	                    'row_alt_start'       => '<tr>',
	                    'row_alt_end'         => '</tr>',
	                    'cell_alt_start'      => '<td>',
	                    'cell_alt_end'        => '</td>',

	                    'table_close'         => '</table>'
	              );

	$this->table->set_template($tmpl);

.. admonition:: Not
	:class: note

	İki "row" bloğu olduğunu farkedeceksiniz. Bunlar satır bilgisinin her tekrarında değişen satır renkleri veya tasarım elementleri oluşturmanıza izin verir.

Tam bir şablon oluşturmanıza gerek YOKTUR. Eğer sadece bazı parçaları değiştirmeye ihtiyacınız varsa, basitçe sadece o elementleri oluşturabilirsiniz. Bu örnekte, sadece tablo açma etiketi değiştiriliyor::

	$tmpl = array ( 'table_open'  => '<table border="1" cellpadding="2" cellspacing="1" class="mytable">' );

	$this->table->set_template($tmpl);

*******************
Fonksiyon Referansı
*******************

$this->table->generate()
========================

Oluşturulan tabloyu içeren bir dizge döndürür. İsteğe bağlı olarak array veya veritabanı sonuç objesi olabilecek bir parametre kabul eder.

$this->table->set_caption()
============================

Tabloya bir caption(=başlık, tablo'nun başlık satırı değil, tablonun üstünde bir başlık) atmanıza izin verir.

::

	$this->table->set_caption('Colors');

$this->table->set_heading()
============================

Bir tablo başlığı ayarlamanıza izin verir. Array veya ayrık parametreler olabilir::

	$this->table->set_heading('Name', 'Color', 'Size');

::

	$this->table->set_heading(array('Name', 'Color', 'Size'));

$this->table->add_row()
========================

Bir satır eklemenize izin verir. Array veya ayrık parametreler olabilir::

	$this->table->add_row('Blue', 'Red', 'Green');

::

	$this->table->add_row(array('Blue', 'Red', 'Green'));

Eğer bir hücreye özellik vermek isterseniz, bu hücreye bir dizide birleştirilmiş özellikleri kullanabilirsiniz. Dizide birleştirilmiş anahtar 'data' değeridir. Diğer key => val ikilileri de tag olarak key = 'val' şeklinde eklenebilir::

	$cell = array('data' => 'Blue', 'class' => 'highlight', 'colspan' => 2);
	$this->table->add_row($cell, 'Red', 'Green');

	// generates
	// <td class='highlight' colspan='2'>Blue</td><td>Red</td><td>Green</td>

$this->table->make_columns()
=============================

Bu fonksiyon tek boyutlu bir arrayi girdi olarak alır ve derinliği arzu edilen sütün genişliğine eşit olan yeni bir array oluşturur. Bu birçok elementi olan tek bir array'in sabit sütün genişliği olan bir tabloda gösterilmesini sağlar. Şu örneğe göz önünde bulundurun:

	$list = array('one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve');

	$new_list = $this->table->make_columns($list, 3);

	$this->table->generate($new_list);

	// Generates a table with this prototype

	<table border="0" cellpadding="4" cellspacing="0">
	<tr>
	<td>one</td><td>two</td><td>three</td>
	</tr><tr>
	<td>four</td><td>five</td><td>six</td>
	</tr><tr>
	<td>seven</td><td>eight</td><td>nine</td>
	</tr><tr>
	<td>ten</td><td>eleven</td><td>twelve</td></tr>
	</table>

$this->table->set_template()
=============================

Şablonunuzu belirlemenize olanak sağlar. Tam veya bir parça şablon kullanabilirsiniz.

::

	$tmpl = array ( 'table_open'  => '<table border="1" cellpadding="2" cellspacing="1" class="mytable">' );

	$this->table->set_template($tmpl);

$this->table->set_empty()
==========================

Boş hücreler için öntanımlı değer atamanıza olanak sağlar. Mesela, boşluk kullanabilirsiniz::

	 $this->table->set_empty("&nbsp;");

$this->table->clear()
=====================

Tablo başlığı ve satır bilgisini temizlemenizi sağlar. Eğer farklı verilerle birden fazla tablo göstermeniz gerekiyorsa, bu işlevi oluşturduğunuz her tablodan sonra önce oluşturulan tabloya ait bilgileri temizlemek için çalıştırmalısınız. Örneğin::

	$this->load->library('table');

	$this->table->set_heading('Name', 'Color', 'Size');
	$this->table->add_row('Fred', 'Blue', 'Small');
	$this->table->add_row('Mary', 'Red', 'Large');
	$this->table->add_row('John', 'Green', 'Medium');

	echo $this->table->generate();

	$this->table->clear();

	$this->table->set_heading('Name', 'Day', 'Delivery');
	$this->table->add_row('Fred', 'Wednesday', 'Express');
	$this->table->add_row('Mary', 'Monday', 'Air');
	$this->table->add_row('John', 'Saturday', 'Overnight');

	echo $this->table->generate();

$this->table->function
======================

Doğal PHP fonksiyonlarının ya da geçerli fonksiyon dizi objelerinin tüm hücrelere uygulanmasına izin verir.

::

	$this->load->library('table');

	$this->table->set_heading('Name', 'Color', 'Size');
	$this->table->add_row('Fred', '<strong>Blue</strong>', 'Small');

	$this->table->function = 'htmlspecialchars';
	echo $this->table->generate();

Yukarıdaki örnekte, bütün hücrelerdeki bilgilere PHP'deki htmlspecialchars() fonksiyonu uygulanır, sonuç ::

	<td>Fred</td><td>&lt;strong&gt;Blue&lt;/strong&gt;</td><td>Small</td>
