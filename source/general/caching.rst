############################
Web Sayfasını Önbelleğe Alma
############################

CodeIgniter maksimum performansa ulaşmanız için sayfaları önbelleğe almanıza olanak tanır.

CodeIgniter oldukça hızlı olmasına rağmen, ekranda gösterdiğiniz dinamik bilginin miktarı sunucu kaynaklarını, hafızayı ve sayfa yükleme hızını etkileyen çevrim işlemlerini arttıracaktır. Sayfanızın görünüşünü olduğu gibi kaydetmesi nedeniyle önbelleğe alarak, neredeyse statik web sayfaların performansına ulaşabilirsiniz.

Önbelleğe Alma Nasıl Çalışır?
=============================

Önbelleğe alma sayfa bazlı olarak kullanabilir ve sayfanın tekrar yenilenmesine kadar geçen süre ayarlayabilirsiniz. Sayfa ilk kez yüklendiğinde, önbellek dosyası system/cache dizinine yazılır. Daha sonra açılan her sayfa için önbellekteki dosya çağrılır ve kullanıcının tarayıcısına gönderilir. Eğer sayfanın süresi geçmişse, silinir ve tekrar tarayıcıya gönderilmeden önce yenilenir.

Not: Benchmark başlıkları önbelleğe atılmaz yani önbelleği kullansanız da sayfa mevcut açılma hızınızı göreceksiniz.

Önbelleği Devreye Alma
======================

Önbelleği devreye almak için, aşağıdaki başlığı controller fonksiyonunda herhangi bir yere koymalısınız::

	$this->output->cache(n);

Burada n, iki yenileme arasında önbellekte kalmasını istediğiniz **dakika** sayısıdır.

Yukarıdaki başlık fonksiyonun herhangi bir yerinde olabilir. Sırası görünümü etkilemeyecektir bu nedenle sizin mantığınıza en uygun yere koyabilirsiniz. Başlık bir kere eklendimi, sayfalarınız önbelleğe atılacaktır.

.. admonition:: ÖNEMLİ
    :class: important

    CodeIgniter'ın çıktı içeriği yükleme tarzı nedeniyle, önbelleğe alma sadece controller ile bir :doc:`view <./views>` dosyası oluşturmanız durumunda çalışır.

.. admonition:: Not 
    :class: note
    
    Önbelleğe alma dosyalarını yazdırmadan önce application/cache dizininin yazılabilir olmasına izin vermelisiniz.

Önbelleğin Silinmesi
====================

Eğer dosylarınızı daha fazla önbelleğe almak istemiyorsanız, controller dosyasındaki ilgili başlığı kaldırmanızın ardından önbellek süresi bittiğinde dosyalar kayıt edilmeyecektir. Not: Başlığın kaldırılmasıyla önbellek dosyaları derhal silinmeyefcektir. Süresi geçtiği zaman silinecektir. Eğer önbelleği daha erken kaldırmak istiyorsanız, önbellek dizini altındaki dosyları elinizle silebilirsiniz.
