###########################
Otomatik Yükleme Kaynakları
###########################

CodeIgniter otomatik yükleme özelliği ile kütüphaneler, helper ve plugin dosylaarını her sistem açılışında otomatik yükleme özelliğine sahiptir. Eğer uygulamanızın genelinde kullanacağınız dosyalara ihtiyacınız varsa, otomatik yükleme opsiyonunu hesaba katmalısınız.

Aşağıdaki maddeler otomatik yüklenir:

- "libraries" dizini altındaki çekirdek sınıfları
- "helpers" dizini altındaki helper dosyları
- "config" dizini altında bulunan kullanıcı ayar dosyaları
- "system/language" dizini altındaki dil dosyaları
- "models" dizini altında bulunan model dosyaları

Kaynakları otomatik yüklemek için, application/config/autoload.php dosyasını aç ve autoload dizisine yükelemek istediğin maddeleri ekle. Gerekli bilgileri, her madde ile ilgili bölümde bulacaksınız.

.. admonition:: Not
    :class: note

    Otomatik yükleme dizinine madde eklerken (.php) dosya uzantısını yazmayın.
