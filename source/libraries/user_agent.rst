##########################
Kullanıcı Bilgileri Sınıfı
##########################

Kullanıcı bilgileri sınıfı ziyaretcinin kullandığı tarayıcı özelliklerini, mobil aygıt özelliklerini elde edebilmenizi veya robot olup olmadığını belirleyebilmenizi sağlar. Ayrıca gönderici (Referrer), dil veya desteklenen karakter seti gibi bilgilerin elde edilebilmesini de kolaylaştırır.

Sınıfın Yüklenmesi
==================

Diğer sınıflar gibi bu sınıf da $this->load->library fonksiyonu ile yüklenir::

	$this->load->library('user_agent');

Sınıf bir kere yüklendikten sonra $this->agent nesnesi üzerinden erişilebilir olacaktır.

Tanımlamalar
============

Tarayıcı, işletim sistemi, ve ziyaretci tipi gibi tanımlamalar application/config/user_agents.php dosyası içerisindedir. Dilerseniz bu dosyayı değiştirerek yeni tarayıcı, isletim sistemi gibi tanımlamalar yapabilirsiniz.

Örnek
=====

Sınıf yüklendiğinde tarayıcı bilgisi, mobil cihaz işletim sistemi veya ziyaretci bir bot ise bot bilgilerini almaya çalışacaktır.

::

	$this->load->library('user_agent');

	if ($this->agent->is_browser())
	{
	    $agent = $this->agent->browser().' '.$this->agent->version();
	}
	elseif ($this->agent->is_robot())
	{
	    $agent = $this->agent->robot();
	}
	elseif ($this->agent->is_mobile())
	{
	    $agent = $this->agent->mobile();
	}
	else
	{
	    $agent = 'Unidentified User Agent';
	}

	echo $agent;

	echo $this->agent->platform(); // Platform info (Windows, Linux, Mac, etc.)

************
Fonksiyonlar
************

$this->agent->is_browser()
===========================

Bir boolean değeri döndürür (TRUE/FALSE) kullanıcının bilinen bir tarayıcıyla mı ziyareti gerçekleştirdiğini sorgular.

::

	if ($this->agent->is_browser('Safari'))
	{
	    echo 'You are using Safari.';
	}
	else if ($this->agent->is_browser())
	{
	    echo 'You are using a browser.';
	}
	

.. not:: Bu örnekte dizi indeksi, tarayıcılı listesinden "Safari" seçildi. Bu listeye application/config/user_agents.php dosyasından erişebilir, eğer isterseniz yeni tarayıcı ekleyebilir ya da mevcudu değiştirebilirsiniz.

$this->agent->is_mobile()
==========================

Bir boolean değeri döndürür (TRUE/FALSE) kullanıcının bilinen bir mobil cihazla mı ziyareti gerçekleştirdiğini sorgular.

::

	if ($this->agent->is_mobile('iphone'))
	{
	    $this->load->view('iphone/home');
	}
	else if ($this->agent->is_mobile())
	{
	    $this->load->view('mobile/home');
	}
	else
	{
	    $this->load->view('web/home');
	}
	

$this->agent->is_robot()
=========================

Bir boolean değeri döndürür (TRUE/FALSE) Ziyaretcinin bir robot olup olmadığını sorgular.

.. not:: Tanım dosyası sadece en polüler robot tanımlarını barındırır. Bu tanımlama bütün robotların bir listesini barındırmaz. Dünyadaki bütün robotların incelenip listeye dahil edilmesi mümkün olmadığı için eğer sitenizi ziyaret eden robotlar tesbit ettiyseniz ve bu robotların da tanımlanmasını istiyorsanız application/config/user_agents.php dosyasını düzenleyerek bu tanımlamayı yapabilirsiniz.

$this->agent->is_referral()
============================

Bir boolean değeri döndürür (TRUE/FALSE) Ziyaretcinin başka bir siteden yönlendirilip yönlendirilmediğini kontrol eder.

$this->agent->browser()
=======================

Ziyarecinin kullandığı tarayıcı adını string olarak döndürür.

$this->agent->version()
=======================

Ziyaretcinin kullandığı tarayıcı versiyonunu string olarak döndürür.

$this->agent->mobile()
======================

Ziyaretcinin kullandığı mobil cihazın adını string olarak döndürür.

$this->agent->robot()
=====================

Ziyaretci robotun adını string olarak döndürür.

$this->agent->platform()
========================

Ziyaretcinin kullandığı işletim sistemi (Platform) adını string olarak döndürür. (Linux, Windows, OS X, vs.).

$this->agent->referrer()
========================

Eğer ziyaretci başka bir siteden yönlendirildiyse basitçe aşşağıdaki metodu kullanarak test edebilirsiniz::

	if ($this->agent->is_referral())
	{
	    echo $this->agent->referrer();
	}

$this->agent->agent_string()
=============================

Kullanıcı bilgilerinin elde edildiği tam metinin salt halini geri döndürür, muhtemelen şuna benzeyecektir::

	Mozilla/5.0 (Macintosh; U; Intel Mac OS X; en-US; rv:1.8.0.4) Gecko/20060613 Camino/1.0.2

$this->agent->accept_lang()
============================

Gönderilen parametreye ait olan dilin kullanıcı tarafından kabul edilip edilmediğini kontrol eder. Örnek ::

	if ($this->agent->accept_lang('en'))
	{
	    echo 'You accept English!';
	}

.. not:: Bu bilginin tamamen doğru olduğu söylenemez, bazı tarayıcılar kabul edilen diller hakkında bilgi göndermeyebilir.

$this->agent->accept_charset()
===============================

Gönderilen parametreye ait olan karakter kodlamasının kullanıcı tarafından kabul edilip edilmediğini kontrol eder. Örnek::

	if ($this->agent->accept_charset('utf-8'))
	{
	    echo 'You browser supports UTF-8!';
	}

.. not:: Bu bilginin tamamen doğru olduğu söylenemez, bazı tarayıcılar karakter kodlaması hakkında bilgi göndermeyebilir.
