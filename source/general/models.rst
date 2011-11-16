######
Model
######

Model, geleneksel MVC yaklaşımını kullanmak isteyenler için **opsiyonel** olarak mevcuttur.

.. contents:: Sayfa İçeriği

Model Nedir?
================

MOdeller veritabanınızla bilgi alışverişi sağlamak üzere tasarlanmış PHP sınıflarıdır. Örneğin, diyelim ki CodeIgniter kullanarak bir blog yönetiyorsunuz. Blog bilgilerinizi ekleme, yenileme ve çekme fonksiyonlarına sajip bir model sınıfınız olmalıdır. Burada bir örnekle model sınıfının neye benzediğini gösterelim::

	class Blogmodel extends CI_Model {

	    var $title   = '';
	    var $content = '';
	    var $date    = '';

	    function __construct()
	    {
	        // Call the Model constructor
	        parent::__construct();
	    }

	    function get_last_ten_entries()
	    {
	        $query = $this->db->get('entries', 10);
	        return $query->result();
	    }

	    function insert_entry()
	    {
	        $this->title   = $_POST['title']; // please read the below note
	        $this->content = $_POST['content'];
	        $this->date    = time();

	        $this->db->insert('entries', $this);
	    }

	    function update_entry()
	    {
	        $this->title   = $_POST['title'];
	        $this->content = $_POST['content'];
	        $this->date    = time();

	        $this->db->update('entries', $this, array('id' => $_POST['id']));
	    }

	}

.. not:: Yukarıdaki örnek :doc:`Active Record <../database/active_record>` veritabanı fonksiyonlarını kullanır.

.. not:: Bu örnekte basitliği korumak için $_POST kullandık. Bu kötü bir uygulamadır ve bir çok genel uygulama :doc:`Bilgi Girişi Sınıfı <../libraries/input>` $this->input->post('title') kullanır.

Bir Modelin Anatomisi
==================

Model sınıfları **application/models/ dizini** altına depolanmıştır. Eğer isterseniz alt-dizinlere de yerleştirebilirsiniz.

Bir model sınıfının taemel yapısı şöyledir::

	class Model_name extends CI_Model {

	    function __construct()
	    {
	        parent::__construct();
	    }
	}

**Model_name** sınıfın ismidir. Sınıf ismi **mutlaka** ilk harfi büyük, sonraki harfleri küçük olmak zorundadır. Sınıfın Model sınıfının extend edilmiş hali olduğundan emin olun.

Dosya adında bütün harfler küçük harf olacaktır. Örneğin, eğer sınıfınız şöyleyse::

	class User_model extends CI_Model {

	    function __construct()
	    {
	        parent::__construct();
	    }
	}

Dosyanız şöyle olacaktır::

	application/models/user_model.php

Model Yüklemek
===============

Modellerin yüklenemsi ve çağrılması :doc:`controller <controllers>` fonksiyonları ile yapılır. MOdel yüklemesi için aşağıdaki fonksiyonu kullanacaksınız::

	$this->load->model('Model_name');

Eğer modeliniz bir alt-sizindeyse, göreli olarak bulunan model dizinini de ekleyiniz. Örneğin, eğer modeliniz application/models/blog/queries.php yolundaysa, yüklerken bunu kullanınız::

	$this->load->model('blog/queries');

Bir defa yüklenince, Model fonksiyonlarınıza aynı sınıfın ismi ile erişebilirisiniz::

	$this->load->model('Model_name');

	$this->Model_name->function();

Eğer farklı bir adla modelinize erişmek isterseniz, yükelem yaparken ikinci parametre olarak bu ismi belirtmelisiniz::

	$this->load->model('Model_name', 'fubar');

	$this->fubar->function();

Buradaki controller örneği modeli yükler, sonra view'e gönderir ::

	class Blog_controller extends CI_Controller {

	    function blog()
	    {
	        $this->load->model('Blog');

	        $data['query'] = $this->Blog->get_last_ten_entries();

	        $this->load->view('blog', $data);
	    }
	}
	

Modeli Otomatik Yükleme
===================

Eğer belirli bir modeli uygulamanızın tamamında kullanıyorsanız, bunu sistem yüklenirken otomatik yükle diyerek CodeIgniter'a diyebilirsiniz. Bunun için **application/config/autoload.php** dosyasını açıp, autoload dizisine eklemeniz gereklidir.

Veritabanına Bağlanmak
===========================

Model yüklendiğinde, veritabanına otomatik olarak **BAĞLANMAZ**. Bağlantı için aşağıdaki opsiyonlar mevcuttur::

-  :doc:`Burada anlatılan<../database/connecting>` standart veritabanı bağlantı metodunu Controller sınıfı ya da Model sınıfından kullanabilirsiniz.
-  Model yükleme fonksiyonunda üçüncü parametreye TRUE değeri vererek, bağlantı bilgilerinizi veritabanı ayar dosyasında tanımladığınız gibi kullanması şartıyla otomatik bağlan diyebilirsiniz:
   ::

	$this->load->model('Model_name', '', TRUE);

-  Üçüncü parametre ile kullanmasını sitediğiniz veritabanı bağlantı ayarlarını elle ayarlayabilirsiniz ::

	$config['hostname'] = "localhost";
	$config['username'] = "myusername";
	$config['password'] = "mypassword";
	$config['database'] = "mydatabase";
	$config['dbdriver'] = "mysql";
	$config['dbprefix'] = "";
	$config['pconnect'] = FALSE;
	$config['db_debug'] = TRUE;

	$this->load->model('Model_name', '', $config);


