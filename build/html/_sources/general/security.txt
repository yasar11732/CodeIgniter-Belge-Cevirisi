########
Güvenlik
########

Bu sayfa web güvenliği hakkında bazı "en iyi uygulamaları" ve CodeIgniter'ın dahili güvenliği konularını içermektedir.

URI Güvenliği
=============

CodeIgniter bozuk verilerin en aza indirgenebilmesi için URI yazımında bazı özel karakterin kullanımını yasaklamıştır, URI yanlızca aşşağıdaki karakterleri içerebilir:

-  Alpha-numeric text
-  Tilde: ~
-  Period: .
-  Colon: :
-  Underscore: \_
-  Dash: -

Register_globals
================

Sistem ön yüklemesi sırasında $_POST ve $_COOKIE değerleri haricindeki bütün global değerler bellekten silinir (unset) Değerlerin bellekten silinmesi register_globals=off tanımlaması ile aynı etkiyi gösterir.

error_reporting
===============

Geliştirme sürecinde, PHP hata raporu kodlarının kapatılması error_reporting sabitine 0 değeri verilmesi ile yapılır. Bu etkisizleştirme, bazı önemli bilgi içeren PHP hatalarının ekrana basılmasını sağlar.

CodeIgniter'ın index.php dosyasındaki **ENVIRONMENT** sabitine **\'production\'** değeri atanınca bu tür hatalar kapatılır. Geliştirme modunda, 'development' değerinin kullanılması tavsiye edilir. İki değer arasındaki fark hakkında detaylı bilgi için  :doc:`Handling Environments <environments>` sayfasını okuyunuz.

magic_quotes_runtime
====================

magic_quotes_runtime tanımlaması sistem önyüklemesi sırasında kapatıldığı için veritabanından gelen kayıtlar içerisindeki slash karakterini ayrıca temizlemenize gerek yoktur.

******************
En iyi uygulamalar
******************

Dışardan gelen her veriyi (POST, COOKIE, URI Verisi veya XML-RPC , SERVER gibi) işlemeden önce aşşağıdaki 3. adımı uygulamaya özen gösterin:

#. Veri doğruluğundan emin olun.
#. Verinin istediğiniz tipte, uzunlukta veya büyüklükte olduğundan emin olun. (Bu adım bazen 1. adımın değişmesine neden olabilir).
#. Veritabanına yazmadan önce verinin özek karakterler veya kodlar içermediğinden emin olun (Sql injection, XSS gibi).

CodeIgniter güvenlik adımların uygulanmasına yardım etmek için araçlar sunar :

XSS Temizleme
=============

CodeIgniter XSS filtrsi ile birlikte dağıtılır. Bu filtre çok kullanılan zararlı kod yerleştirme tekniğine karşı, hijack ve diğer zararlı kodlara karşı kontroller yapar. XSS filtresi hakkında detaylı açıklamaya :doc:`buradan <../libraries/security>` erişin.

Veri doğrulama
==============

CodeIgniter verilerinizi doğrulamada, filtrlemede ve uygun hale getirmenizde yardımcı olacak :doc:`veri doğrulama <../libraries/form_validation>` araçlarıyla birlikte gelir.

Veritabanına eklemeden önce bütün verilerin kontrolü ve filtrelenmesi
=====================================================================

Asla dış kaynaklı verileri filtrlemeden veritabanına eklemeyin. Bu konu hakkında daha fazla bilgiye :doc:`buradan <../database/queries>` ulaşabilirsiniz.
