#######################
Uygulamanızı Ayrımlayın
#######################

Ayrımlama sınıfı karşılaştırma sonuçlarını, çalıştırdığınız sorguları and $_POST bilgilerini sayfanın altında gösterecektir. Bu bilgiler uygulama geliştirme ve optimize etme sırasında yardımcı olurlar.

Sınıfın Başlatılması
====================

.. önemli:: Bu sınıfın başlatılmasına gerek YOKTUR. Bu sınıf :doc:`Output Sınıfı <../libraries/output>` ile birlikte otomatik olarak yüklenir. Eğer ayrımlama etkinleştirilmişse aşağıda görülür.

Ayrımlamayı Etkinleştirme
=========================

Ayrımlamayı etkinleştirmek için :doc:`Controller <controllers>` fonksiyonlarının herhangi bir yerine şu başlığı ekleyebilirsiniz::

	$this->output->enable_profiler(TRUE);

Eklendikten sonra ekrana gelen sayfanın alt kısmına rapor gelecektir.

Etkisizleştirme için::

	$this->output->enable_profiler(FALSE);

Karşılaştırma Noktaların Ayarları
=================================

Ayrımlama değerlerini derlemek ve karşılaştırma bilgilerini göstermek için belirli imla ile noktalar tanımlamalısınız.

Lütfen karşılaştırma noktaları için :doc:`Benchmark - Karşılaştırma Sınıfı  <../libraries/benchmark>`sayfasında anlatılanları okuyun.

Ayrımlama Sınıfını Etkinleştirme ve Etkisizleştirme
===================================================

Ayrımlama sınıfını her yerde etkinleştirme ve etkisizleştirme için ilgili değişken ayarları TRUE ya da FALSE yapılabilir. Bu iki yol ile olur. Birincisi, uygulmanın application/config/profiler.php dosyasındaki config değişkenlerini ayarlama

::

	$config['config']          = FALSE;
	$config['queries']         = FALSE;

Controller dosyanızda, varsayılan değerlerin üzerine :doc:`Output sınıfınfaki <../libraries/output>` set_profiler_sections() metodunu çağırarak yazma::

	$sections = array(
	    'config'  => TRUE,
	    'queries' => TRUE
	    );

	$this->output->set_profiler_sections($sections);

Kullanılabilecek bölümler ve anahtar diziler aşağıdaki tabloda tarif edilmiştir.

======================= =================================================================== ==========
Anahtar                 Tanım                                                         		Varsayılan
======================= =================================================================== ==========
**benchmarks**          Karşılaştırma noktalarında ve toplamda geçen süre					TRUE
**config**              CodeIgniter Config değişkenleri										TRUE
**controller_info**     Controller sınıfı ve çağrılan metodu								TRUE
**get**                 İstemdeki bütün GET bilgileri										TRUE
**http_headers**        Yapılan istem için HTTP header değerleri							TRUE
**memory_usage**        Yapılan istem için harcanılan hafıza değeri, bayt olarak			TRUE
**post**                İstemdeki bütün POST bilgileri										TRUE
**queries**             Çalıştırılan bütün veritabanı sorguları ve çalışma süreleri			TRUE
**uri_string**          Yapılan istemdeki URI değerleri										TRUE
**session_data**        Oturumda saklanılan bilgiler										TRUE
**query_toggle_count**  Görünmez sorgu bloklarının ardından yapılan toplam sorgu  sayısı	25
======================= =================================================================== ==========