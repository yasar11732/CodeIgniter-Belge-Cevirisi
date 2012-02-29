#######################
Alışveriş Sepeti Sınıfı
#######################

Bu sınıf kullanıcılar siteyi ziyaret ederken ürünlerin eklenebileceği, "session"da saklanan bir alışveriş sepeti oluşturmamız için bize yardım eder. Alışveriş sepetindeki ürünlerin silinmesi, miktarının değiştirlmesi veya yeni ürün eklenmesi gibi işlemlere olanak sağlar.

Sınıf standart alışveriş sepeti fonksiyonlarını yerine getirir, kredi kartı ödemesi gibi ödeme fonksiyonlarını barındırmaz.

.. contents:: Page Contents

Alışveriş Sepeti Sınıfının hazılanması
======================================

.. admonition:: Önemli
	:class: important 

	Alışveriş sepeti sınıfı sepet bilgilerini veritabanına saklamak için CodeIgniter'in :doc:`Session Sınıfını <sessions>` kullanır, bu yüzden alışveriş sepeti sınıfını kullanmadan önce :doc:`Session Dokümantasyonunda<sessions>` anlatılan veritabanlarını ayarlayıp appliction/config/config.php dosyasındaki session özellikleri kısmını düzenlemeniz gerekmektedir.

Sınıfı hazılamak için $this->load->library fonksiyonu ile yüklenmesi gerekmektedir ::

	$this->load->library('cart');

Yüklendikten sonra Cart nesnesi $this->cart altında kullanılabilir olacaktır::
 	
	$this->cart

.. admonition:: Not
	:class: note

	Alışveriş sepeti sınıfı otomatik olarak Session sınıfını belleğeye yükleyeceği için, uygulamanızın herhangi bir yerinde session sınıfını elle yüklemeden kullanabilirsiniz.

Alışveriş sepetine ürün eklenmesi
=================================

Alışveriş sepetine ürün eklemek için basitce aşşağıda görünen tipte bir array $this->cart->insert() fonksiyonuna parametre olarak gönderilir. Örnek ::

	$data = array(
	               'id'      => 'sku_123ABC',
	               'qty'     => 1,
	               'price'   => 39.95,
	               'name'    => 'T-Shirt',
	               'options' => array('Size' => 'L', 'Color' => 'Red')
	            );

	$this->cart->insert($data);

.. admonition:: Önemli
	:class: important

	İlk dört anahtar (id, qty, price, ve name) **zorunlu** anahtarlardır, eğer bu 4 anahtardan biri hatalı ise ürün sepete eklenmeyecektir. Beşinci anahtar (options) isteğe bağlı olarak kullanılabilir, ürüne ait ekstra bilgiler bu alanda saklanabilir.

Anahtarlar ve açıklamaları:

-  **id** - Ürünün tekil tanımlayıcısı. Her ürün için tekil olan kod, bir çok kişi bunu stok kodu olarak kullanmaktadır.
   Tipik oalrak "sku" ya da bunun gibi başka bir tanımlayıcı.
-  **qty** - Ödenecek ürün miktarı. (Miktar).
-  **price** - Ürünün fiyatı.
-  **name** -  Ürün adı.
-  **options** - Herhangi ekstra bilgi, dizi (array) olarak eklenmelidir.

Ek olarak 2 adet rezerve kelime mevcuttur, bunlar : rowid ve subtotal kelimeleridir. Alışveriş sepeti sınıfı içinde kullanıldığı için sepete ürün eklenirken bu kelimeleri anahtar tanımlayıcısı olarak kullanmamaya özen gösterin.

Sepet sınıfına parametre olarak gönderilen dizi başka verilerde içerebilir, bu veriler session'da saklanacaktır. BUnunla birlikte, ürün bilgilerini ekranda tablolama ile gösterimin en kolay yolu, listelenecek ürün bilgilerinin tamamının tutulmasıdır.

Eğer bir ürün sepete eklenirse, insert() metodu $rowid değerini geri döndürecektir.

Çoklu ürün girişi
=================

Bir seferde birden falza ürünü sepete ekleyebilirsiniz, bunun için çok boyutlu bir array'ın aşşağıdaki örnekte olduğu gibi tanımlanması gerekmetedir. Bu yöntem kullanıcının tek sayfada çoklu ürün seçimi yaptığı ve sepete eklediği anlarda kullanışlı olacaktır.

::

	$data = array(
	               array(
	                       'id'      => 'sku_123ABC',
	                       'qty'     => 1,
	                       'price'   => 39.95,
	                       'name'    => 'T-Shirt',
	                       'options' => array('Size' => 'L', 'Color' => 'Red')
	                    ),
	               array(
	                       'id'      => 'sku_567ZYX',
	                       'qty'     => 1,
	                       'price'   => 9.95,
	                       'name'    => 'Coffee Mug'
	                    ),
	               array(
	                       'id'      => 'sku_965QRS',
	                       'qty'     => 1,
	                       'price'   => 29.95,
	                       'name'    => 'Shot Glass'
	                    )
	            );

	$this->cart->insert($data);

Sepetin gösterilmesi
====================

Ürün gösterimi için aşşağıdaki örneğe benzer bir :doc:`view dosyası </general/views>` oluşturmamız gerekmedir.

Bu örneğin :doc:`form helper </helpers/form_helper>` kullandığını unutmayın.

::

	<?php echo form_open('path/to/controller/update/function'); ?>

	<table cellpadding="6" cellspacing="1" style="width:100%" border="0">

	<tr>
	  <th>QTY</th>
	  <th>Item Description</th>
	  <th style="text-align:right">Item Price</th>
	  <th style="text-align:right">Sub-Total</th>
	</tr>

	<?php $i = 1; ?>

	<?php foreach ($this->cart->contents() as $items): ?>

		<?php echo form_hidden($i.'[rowid]', $items['rowid']); ?>

		<tr>
		  <td><?php echo form_input(array('name' => $i.'[qty]', 'value' => $items['qty'], 'maxlength' => '3', 'size' => '5')); ?></td>
		  <td>
			<?php echo $items['name']; ?>

				<?php if ($this->cart->has_options($items['rowid']) == TRUE): ?>

					<p>
						<?php foreach ($this->cart->product_options($items['rowid']) as $option_name => $option_value): ?>

							<strong><?php echo $option_name; ?>:</strong> <?php echo $option_value; ?><br />

						<?php endforeach; ?>
					</p>

				<?php endif; ?>

		  </td>
		  <td style="text-align:right"><?php echo $this->cart->format_number($items['price']); ?></td>
		  <td style="text-align:right">$<?php echo $this->cart->format_number($items['subtotal']); ?></td>
		</tr>

	<?php $i++; ?>

	<?php endforeach; ?>

	<tr>
	  <td colspan="2"> </td>
	  <td class="right"><strong>Total</strong></td>
	  <td class="right">$<?php echo $this->cart->format_number($this->cart->total()); ?></td>
	</tr>

	</table>

	<p><?php echo form_submit('', 'Update your Cart'); ?></p>
	
Sepeti güncellemek
==================

Sepeti güncellemek için $this->cart->update() fonksiyonuna Row ID ve quantity anahtarlarını içeren bir array parametre olarak gönderilmelidir.

.. admonition:: Not
	:class: note

	quantity anahtarı ürünün miktarını tanımlar ve sıfır olarak ayarlanırsa ürün septten kaldırılır.

::

	$data = array(
	               'rowid' => 'b99ccdf16028f015540f341130b6d8ec',
	               'qty'   => 3
	            );

	$this->cart->update($data); 

	// Or a multi-dimensional array

	$data = array(
	               array(
	                       'rowid'   => 'b99ccdf16028f015540f341130b6d8ec',
	                       'qty'     => 3
	                    ),
	               array(
	                       'rowid'   => 'xw82g9q3r495893iajdh473990rikw23',
	                       'qty'     => 4
	                    ),
	               array(
	                       'rowid'   => 'fh4kdkkkaoe30njgoe92rkdkkobec333',
	                       'qty'     => 2
	                    )
	            );

	$this->cart->update($data);

Row ID nedir?
*************

Sepet sınıfı her eklenen ürün için tekil bir tanımlayıcı tayin eder, row ID bu tanımayıcıya verilen isimdir. Row id yardımı ile sepet içindeki ürünler tek tek düzenlenebilir.

Örnekle açıklamak gerekirse : Bir müşterimiz 2 adet tişört satın aldı, Ürün kodu aynı fakat bedenleri farklı. Ürün kodu fiyat, adet bilgileri aynı olduğu halde ürünlerin bedenleri farklı. Bizim iki ürünüde birbirinden bağımsız olarak yönetebilmemiz gerekiyor, CodeIgniter bunu sepetteki her farklı ürün için tekil rowid tanımlayarak mümkün kılıyor, rowid üretilirken ürün kod ve options parametresindeki değerleri kullanılıyor.

Nerdeyse bütün işlemlerin sepet gösterimi sayfasında yapılacağını düşünürsek, rowid değerini bolca kullanacağımızı görebiliriz. Sepet üzerinde değişiklik yapabilmemiz için sepeti gösterirken rowid değerini gizli eleman (hidden field) olarak forma dahil etmemiz gerekecektir. Sepeti güncellerken de rowid değişkenini update fonksiyonuna gönderdiğimizden emin olmamız gerekmektedir. Daha fazla bilgi için sepetin gösterilmesi bölümüne göz atınız.

Fonksiyon Referansları
======================

$this->cart->insert();
**********************

Sepete yeni bir ürün eklemenizi sağlar.

$this->cart->update();
**********************

Sepetteki ürün veya ürünleri güncellemenizi sağlar.

$this->cart->remove(rowid);
***************************

Allows you to remove an item from the shopping cart by passing it the rowid.

$this->cart->total();
*********************

Sepetin toplam tutarını, yani alt toplamını verir.

$this->cart->total_items();
***************************

Sepetteki toplam ürün adetini verir.

$this->cart->contents(boolean);
*******************************

Returns an array containing everything in the cart. You can sort the order,
by which this is returned by passing it "true" where the contents will be sorted
from newest to oldest, by leaving this function blank, you'll automatically just get
first added to the basket to last added to the basket.

$this->cart->has_options(rowid);
********************************

Eğer ürünün options değişkeni bir değer içeriyorsa TRUE döndürür. Bu fonksyion $this->cart->contents() ile kullanılan döngüler içinde kullanılmak üzere tasarlanmıştır, bu yüzden rowid değerini “Sepetin gösterilmesi” başlığı altındaki gibi kullanmalısınız.

$this->cart->product_options(rowid);
************************************

Seçilen ürüne özel opstions dizisini geri döndürür. Bu fonksiyon yukarıda örneği verildiği gibi, rowid değeri belirlendiğinde, $this->cart->contents() fonksiyonu kullanılarak döngü içinde tasarlanır.

$this->cart->destroy();
***********************

Sepeti yok etmek için kullanılır. Müşteri ödemeyi gerçekleştirdikten sonra kullanmak isteyebilirsiniz.
