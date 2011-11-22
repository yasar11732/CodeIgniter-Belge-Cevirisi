##############
Hata İşleme
##############

CodeIgniter aşağıdaki fonksiyonların uygulamalarınızda kullanılması durumunda hata raporlamasına izin verir. Ayrıca, hata kayıt sınıfı ile hata ayıklama mesajlarını yazı dosyası olarak kayıt da edebilirsiniz.

.. not:: CodeIgniter aksi belirtilmediği sürece bütün PHP hatalarını gösterir. Bu özelliği, program geliştirmeniz bitince değiştirmek isteyebilirsiniz. Kök dizinde bulunan index.php dosyasının başındaki error_reporting() fonksiyonu bulacaksınız. Hata raporlamanın pasif hale getirilmesi, hataların olması durumunda kayıt edilmesini ÖNLEMEZ.

CodeIgniter'daki birbirinden farklı çoğu sistem, uygulama boyunca oluşan hataları basit bir arayüzle yansıtır. Bu yaklaşım hata mesajlarının sınıf/fonksiyon ayrımına girmeden toplanmasına izin verir.

Aşağıdaki fonksiyonlar hata üretmenize izin verir:

show_error('message' [, int $status_code= 500 ] )
===================================================

Bu fonksiyon, aşağıdaki hata şablonu kullanılarak hataları gösterir :

application/errors/error_general.php

Seçime bağlı parametre $status_code, hata ile hangi HTTP kodunun gönderildiğini belirler.

show_404('page' [, 'log_error'])
==================================

Bu fonksiyon, aşağıdaki hata şablonu kullanılarak 404 hatalarını gösterir :

application/errors/error_404.php

Bu fonksiyon, sayfa bulunamazsa dosya yoluna göndereceği yazıyı bekler. Not: CodeIgniter eğer controller bulunamazsa 404 mesajlarını otomatik olarak oluşturur.

CodeIgniter show_404() çağrılarını otomatik olarak kayıt eder. İkinci parametreyi FALSE edilrse kayıt tutması önlenir.

log_message('level', 'message')
================================

Bu fonksiyon log dosyasına mesaj yazmanıza izin verir. İlk parametre hangi tip mesaj olduğunu (debug, error, info), ikinci parametre de mesajın kendisini gösterir. Örneğin::

	if ($some_var == "")
	{
	    log_message('error', 'Some variable did not contain a value.');
	}
	else
	{
	    log_message('debug', 'Some variable was correctly set');
	}

	log_message('info', 'The purpose of some variable is to provide some value.');

Üç farklı mesaj tipi vardır:

#. Error Mesajları. Bunlar PHP ya da kullanıcı hataları gibi olan gerçek hatalardır.
#. Debug Mesajları. Bu mesajlar hata ayıklamaya yardımcı mesajlardır. Örneğin, eğer bir sınıf başlatıldıysa, log dosyasına bu bilgiyi ekleyebilirsiniz.
#. Informational Mesajları. Bunlar düşük öncelikli, sadece işlem sırasında oluşan bilgilerdir. CodeIgniter doğal olarak herhangi bir bilgi mesajı oluşturmaz ama siz uygulamanızda olmasını isteyebilirsiniz.

.. not:: Log dosyasına yazdırmak için, "log" dizininiz yazılabilir olmalıdır. Ayrıca, kayıt için "eşiği" ayarlamalısınız. Örneğin, sadece hata (error) mesajlarını istiyor ve diğer iki tipi istemiyor olabilirsiniz. Eğer kayıtlama istemiyorsanız sıfır değerini girmelisiniz.
