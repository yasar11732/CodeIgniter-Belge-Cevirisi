#####
View
#####

View basit bir web sayfası, ya da başlık, alt-başlık, kenar-kolonu gibi sayfa parçasıdır. Gerçekte, eğer bir hiyerarşiye ihtiyacınız varsa, View dosyaları diğer View dosyaları içine de esnek olarak gömülebilirler.

View dosyaları, asla doğrudan çağrılmaz, :doc:`controller <controllers>` dosyaları tarafından yüklenirler. MVC çatısını hatırlarsak, Controller bir trafik polisi gibi davranıp, belirli View dosyalarını getirirler. Eğer :doc:`Controllers <controllers>` sayfasını okumadıysanız, devam etmeden önce okuyunuz.

Hadi :doc:`controller <controllers>` sayfasında hazırladığınız örnek controller dosyasını kullanıp, ona bir View ekleyelim.

View Oluşturma
===============

Metin düzenleyicinizi kullanarak, blogview.php dosyasını oluşturup, içine şunları ekleyelim::

	<html>
	<head>
		<title>My Blog</title>
	</head>
	<body>
		<h1>Welcome to my Blog!</h1>
	</body>
	</html>
	
Sonra da application/views/ dizini altına kayıt edelim.

View Yükleme
==============

Belirli View dosyalarını yüklemek için aşağıdaki fonksiyonu kullanmalısınız::

	$this->load->view('name');

Burada name View dosyasının adıdır. Not: Eğer dosyanızın uzantısı .php ise, dosya uzantısını yazmanıza gerek yoktur.

Şimdi, daha önce blog.php ismi ile oluşturduğumuz dosyayı, controller içinden açarak echo ile yazdırdığımız satırı view yükleme fonksiyonu ile değiştirelim::

	<?php
	class Blog extends CI_Controller {

		function index()
		{
			$this->load->view('blogview');
		}
	}
	?>

Eğer daha önce geçen URL adresini kullanarak sitenizi ziyaret ederseniz, yeni görüntüyü görebilirsiniz. BUnun için URL aynısıdır::

	example.com/index.php/blog/

Çoklu view yükleme
======================

CodeIgniter bir controller $this->load->view ile çoklu yükleme kabiliyetine sahiptir. Eğer birden fazla çağrı olursa, hepsi birbirine eklenecektir. Örneğin, başlık görüntüsü, menü görüntüsü, içerik görüntüsü ve alt-başlık görüntüsünü göstermekmek istediğinizde, şuna benzer bir şey olur ::

	<?php

	class Page extends CI_Controller {

	   function index()
	   {
	      $data['page_title'] = 'Your title';
	      $this->load->view('header');
	      $this->load->view('menu');
	      $this->load->view('content', $data);
	      $this->load->view('footer');
	   }

	}
	?>

Yukarıdaki örnekte, aşağıda anlatılan "dinamik bilgi eklemeyi" kullandık.

Alt-dizinlere View depolama
================================

Eğer organizayonda tercih ederseniz, View dosylarınız alt-sizinlerde de depolanabilir. Bu durumda ihtiyacınız, view yüklenirken dizin adını da belirtmektir. Örneğin::

	$this->load->view('folder_name/file_name');

View'lara Dinamik Bilgi Eklemek
===============================

Bilgiler View yüklemede kullanılan fonksyionun ikinci parametresi olarak, Controller içinden bir **dizi** ya da bir **obje** kullanılarak aktarılır. Burada dizi ile aktarımın örneği::

	$data = array(
	               'title' => 'My Title',
	               'heading' => 'My Heading',
	               'message' => 'My Message'
	          );

	$this->load->view('blogview', $data);

ve burada da obje kullanılarak aktarım örneği::

	$data = new Someclass();
	$this->load->view('blogview', $data);

Not: Eğer obje kullanılarak aktarım yapılıyorsa, sınıf değişkenleri dizi elemanlarına dönüştürülmelidir.

Hadi Controller dosyanızda deneyin. Açıp şu kodu ekleyelim::

	<?php
	class Blog extends CI_Controller {

		function index()
		{
			$data['title'] = "My Real Title";
			$data['heading'] = "My Real Heading";

			$this->load->view('blogview', $data);
		}
	}
	?>

Şimdi view dosyanızı açın ve metin değişkenlerini, ilgili dizinin anahtarları olacak şekilde değiştirin::

	<html>
	<head>
		<title><?php echo $title;?></title>
	</head>
	<body>
		<h1><?php echo $heading;?></h1>
	</body>
	</html>

Sonra sayfayı, kullandığınız URL ile yüklediğinizde değişkenlerin yerini aldığını göreceksiniz.

Döngüler Oluşturmak
==============

View dosylarına gönderilen değişkenler basit değişkenlerle sınırlı değildirler. Çoklu starılarla döngülere alabileceğiniz çok boyutlu diziler gönderebilirsiniz. Örneğin, veritabanından çektiğiniz bilgiler, çok boyutlu dizilere tipik birer örnektirler.

Basit bir örnek. Bunu controller dosyasına ekleyin::

	<?php
	class Blog extends CI_Controller {

		function index()
		{
			$data['todo_list'] = array('Clean House', 'Call Mom', 'Run Errands');

			$data['title'] = "My Real Title";
			$data['heading'] = "My Real Heading";

			$this->load->view('blogview', $data);
		}
	}
	?>

Şimdi View dosyasını açın ve bir döngü oluşturun::

	<html>
	<head>
		<title><?php echo $title;?></title>
	</head>
	<body>
		<h1><?php echo $heading;?></h1>
	
		<h3>My Todo List</h3>

		<ul>
		<?php foreach ($todo_list as $item):?>
	
			<li><?php echo $item;?></li>
	
		<?php endforeach;?>
		</ul>

	</body>
	</html>

.. Not:: Yukarıdaki örnekte fark edeceğiniz gibi PHP'nin alternatif imlasını kullandık. Eğer bu imlaya yabancı iseniz, bu konu hakkında :doc:`here </general/alternative_php>` okuyun.	

View'den Bilgi Geri Dönüşü
=======================

Fonksiyonun üçüncü **opsiyonel** parametresi, tarayıcıya bilgiyi göndermektense metin olarak geri dönmeye izin verir. Bilgiyi başka yollardan işlemek isterseniz, bu yöntem oldukça kullanışlıdır. Eğer bu parametreyi true (boolean) yaparsanız, beli geriye dönecektir. Varsayılan değer tarayıcıya bilgi yazdıran false değeridir. Eğer bilgi geri geliyorsa, bunun bir değişkene atanması gerekitğini unutmayın::

	$string = $this->load->view('myfile', '', true);

