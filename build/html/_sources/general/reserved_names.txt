###############
Rezerve İsimler
###############

CodeIgniter kendi operasyonlarında fonksiyon ve isim serisi kullanır. Bunun nedeni, bazı isimler geliştiriciler için kullanılmamalıdır. Aşağıdaki liste kullanılmaması gereken rezerve isim listesidir.

Controller isimleri
-------------------

Controller sınıfınız ana controller uygulamasından genişletilmesi durumunda, kendi sınıfınıza vereceğiniz isimlere dikkat etmelisiniz, yoksa sınıflar kendi lokal sınıflarınızın üzerine yazılır. Aşağıdaki liste kullanılmaması gereken rezerve isim listesidir. Controller fonksiyonları içinde bu isimler kullanılmamalıdır:

-  Controller
-  CI_Base
-  _ci_initialize
-  Default
-  index

Fonksiyonlar
------------

-  is_really_writable()
-  load_class()
-  get_config()
-  config_item()
-  show_error()
-  show_404()
-  log_message()
-  _exception_handler()
-  get_instance()

Değişkenler
-----------

-  $config
-  $mimes
-  $lang

Sabitler
--------

-  ENVIRONMENT
-  EXT
-  FCPATH
-  SELF
-  BASEPATH
-  APPPATH
-  CI_VERSION
-  FILE_READ_MODE
-  FILE_WRITE_MODE
-  DIR_READ_MODE
-  DIR_WRITE_MODE
-  FOPEN_READ
-  FOPEN_READ_WRITE
-  FOPEN_WRITE_CREATE_DESTRUCTIVE
-  FOPEN_READ_WRITE_CREATE_DESTRUCTIVE
-  FOPEN_WRITE_CREATE
-  FOPEN_READ_WRITE_CREATE
-  FOPEN_WRITE_CREATE_STRICT
-  FOPEN_READ_WRITE_CREATE_STRICT

