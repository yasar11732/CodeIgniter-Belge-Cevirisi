#############
Output Sınıfı
#############

Çıktı sınıfı sadece birkaç fonksiyon içeren küçük bir sınıftır: Hazırlanmış olan web sayfası çıktısının istek yapan tarayıcıya gönderilmesini sağlar. Ayrıca web sayfalarınızı önbeleğe almak (bkz: :doc:`Caching-Önbelleğe alma<../general/caching>`) için de kullanılır.

.. admonition:: Not
    :class: note

    Bu sınıf sistem tarafından otomatik yüklendiği için ayrıca elle yüklemenize gerek yoktur.

Normal durumlarda sınıf otomatik olarak yüklenir ve arka planda sizinin herhangi bir şey yapmanıza gerek kalmadan çalışır. Örnek olarak :doc:`Yükleyici sınıfı<../libraries/loader>` ile bir view yüklediğinizde view dosyası otomatik olarak Codeigniter'in son işlemi olarak çıktı sınıfına yönlendirilir. İşi CodeIgniter'a bırakmayıp çıktıya müdahale etmek istediğinizde aşşağıdaki iki fonksiyonu kullanabilrsiniz.

$this->output->set_output();
=============================

Son çıktıyı bir string değerinden göndermenize olanak sağlar. Örnek kullanım ::

	$this->output->set_output($data);

.. admonition:: Önemli
    :class: important

    Eğer çıktıyı elle gönderiyorsanız, çıktıyı gönderen kod çağrılan fonksiyonun en sonunda bulunmalıdır. Örnek olarak, eğer control dosyalarınız içerisinde bir sayfa yaratılıyorsa çıktıyı gönderen kod bu fonksiyonun en sonunda bulunmalıdır.

$this->output->set_content_type();
====================================

Çıktıyı sayfanızın mime-type formatını belirterek göndermenizi sağlar. Böylece, JSON data, JPEG, XML formatları kolayca kullanabilirsiniz.

::

	$this->output
	    ->set_content_type('application/json')
	    ->set_output(json_encode(array('foo' => 'bar')));

	$this->output
	    ->set_content_type('jpeg') // You could also use ".jpeg" which will have the full stop removed before looking in config/mimes.php
	    ->set_output(file_get_contents('files/something.jpg'));

.. admonition:: Önemli
    :class: important

    Tipi belli olmayan sayfalar için config/mimes.php dosyasında tanımlı olmadığından emin olun.

$this->output->get_output();
=============================

Herhangi bir zamanda çıktı sınıfının sahip olduğu bütün çıktı verilerini almanıza yardımcı olur. Örnek kullanım ::

	$string = $this->output->get_output();

Bu fonksiyonun değer döndürebilmesi için çıktı sınıfına daha önceden çıktı verisi gönderilmiş olmalıdır. Örnek olarak $this->load->view() fonksiyonu view dosyamızı işleyerek çıktı sınıfına çıktı verisi olarak gönderecek bir fonksiyondur.

$this->output->append_output();
================================

Çıktı verilerine bilgi ekler. Örnek kullanımı::

	$this->output->append_output($data);

$this->output->set_header();
=============================

Başlığın (Header) elle yapılandırılmasına olanak sağlar. Bu fonksiyon ile atanan header çıktıları, çıktı sınıfı tarafından header verileri gönderilirken kullanılır. Örneğin::

	$this->output->set_header("HTTP/1.0 200 OK");
	$this->output->set_header("HTTP/1.1 200 OK");
	$this->output->set_header('Last-Modified: '.gmdate('D, d M Y H:i:s', $last_update).' GMT');
	$this->output->set_header("Cache-Control: no-store, no-cache, must-revalidate");
	$this->output->set_header("Cache-Control: post-check=0, pre-check=0");
	$this->output->set_header("Pragma: no-cache");

$this->output->set_status_header(code, 'text');
=================================================

Server durumu başlığını (Server status header) elle yapılandırmak için kullanılır. Örnek ::

	$this->output->set_status_header('401');
	// Sets the header as:  Unauthorized

`Başlıkların tam listesi için <http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html>`_ buraya bakınız.

$this->output->enable_profiler();
==================================

:doc:`Profiler  <../general/profiling>` kullanılıp kullanılmayacağını bu fonksiyon ile belirleyebilirsiniz. Profiler kullanmak için :doc:`Controller <../general/controllers>` foksiyonunuzun herhangi bir yerine aşağıdaki kodu uygulayabilirsiniz::

	$this->output->enable_profiler(TRUE);

Bu komut ile uygulama profili aktif hale gelecek ve oluşturulan rapor sayfanızın en altına eklenecektir.

Uygulama profilini devre dışı bırakmak için ::

	$this->output->enable_profiler(FALSE);

$this->output->set_profiler_sections();
=========================================

Profiler uygulamasını belirli kısımlar için devreye alınmasına ve kapatılmasına izin verir. Daha fazla bilgi için lütfen  :doc:`Profiler <../general/profiling>` bölümünü okuyunuz.

$this->output->cache();
=======================

Çıktı kütüphanesi ayrıca Caching kontrolü de yapmaktadır. Daha fazla bilgi için :doc:`caching - önbelleğe alma <../general/caching>` konusuna gözatabilirsiniz.

Parsing Execution Variables
===========================

CodeIgniter sözde değişkenlerden (pseudo-variables) {elapsed_time} ve {memory_usage} değişkenlerini varsayılan değişkenler olarak çıktıda ayrıştırır. Bunu devre dışı bırakmak için controller dosyanızda $parse_exec_vars sınıf özelliğini FALSE yapınız::

	$this->output->parse_exec_vars = FALSE;

