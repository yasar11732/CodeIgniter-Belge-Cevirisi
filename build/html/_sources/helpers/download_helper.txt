#########################
İndirme (Download) Helper
#########################

İndirme Helper gönderilen verinin bilgisayara dosya şeklinde kaydedilmesine olanak sağlar.

.. contents:: Sayfa İçeriği

Helper Yükleme
==============

Helper aşağıdaki kodla yüklenir

::

	$this->load->helper('download');

Kullanılabilen fonksiyonlar aşağıdadır :

force_download('filename', 'data')
==================================

osyanın indirilebilmesi için başlığın (Header) oluşturulmasını sağlar. İlk parametre kullanıcı dosyayı kaydederken göreceği geçerli dosya adıdır (Kullanıcı dosya adını değiştirmezse sizin belirlediğiniz bu isimle kayıt edilecektir). İkinci parametre dosya içeriğidir. Örnek:

::

	$data = 'Here is some text!';
	$name = 'mytext.txt';
	force_download($name, $data);

Var olan bir dosyanın indirilebilmesi için öncelikle dosya içeriğinin $data değerine yüklenmesi gerekmektedir:

::

	$data = file_get_contents("/path/to/photo.jpg"); // Dosya içeriğini oku
	$name = 'myphoto.jpg';
	force_download($name, $data);

