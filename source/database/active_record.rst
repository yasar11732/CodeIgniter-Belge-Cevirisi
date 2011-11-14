###################
Active Record Sınıfı
###################

CodeIgniter Active Record Veritabanı Şablonu'nun düzenlenmiş şeklini
kullanır. Bu şablon küçük betikler ile veritabanınıza ekleme, güncelleme
ve çekme işlemleri için bilgi sağlar. Bazen kodun sadece bir veya iki gerekli 
satırı ile veritabanı işlemi gerçekleştirebilir. CodeIgniter her veritabanı
tablosu için bir sınıf dosyasına ihtiyaç duymaz. Bunu sağlamak yerine
daha basit bir arayüzü vardır.

Basitliğin ötesinde, Active Record kullanmanın önemli yararı veritabanı sorgularını 
uyarladığından veritabanından bağımsız uygulamalar geliştirmenize olanak sağlar.
Üstelik bu sistemden kaçan sorguları bertaraf ederek güvenli sorgular oluşturmanızı sağlar.

.. note:: Eğer kendi sorgularınızı yazmak istiyorsanız bu sınıfı veritabanı ayar dosyasından
		etkisiz hale getirebilirsiniz. Bu sayede veritabanı kütüphaneniz ve uyarlayıcınız
		daha az kaynak kullanacaktır.
		
.. contents:: Sayfa İçeriği

**************
Veri Çekmek
**************

Bu kısımdaki fonksiyonlar **SELECT** SQL komutunu oluşturacaktır.

$this->db->get()
================

Seçili sorguyu çalıştırır ve sonuçları döndürür. Tek başına bir tablodan tüm kayıtları 
çekebilirsiniz::

	$query = $this->db->get('tablom');  // İşlem Sonrası: SELECT * FROM tablom

İkinci ve üçüncü değeri ise limit ve offset ayarlarını etkin hale getirmenizi
sağlar::

	$query = $this->db->get('tablom', 10, 20);
	// İşlem Sonrası: SELECT * FROM tablom LIMIT 20, 10 (MySQL kullanırken. Diğer veritabanlarında biraz farklıdır.)

Sonuçları $query gibi bir isme sahip bir değişkene atamaya dikkat etmelisiniz ki
sonuçları kullanabilesiniz::

	$query = $this->db->get('tablom');
	
	foreach ($query->result() as $row)
	{
		echo $row->title;
	}

Lütfen :doc:`result functions <results>` sayfasını tam dökümantasyon için ziyaret ediniz.

$this->db->get_compiled_select()
================================

Bu fonksiyon `$this->db->get()` fonksiyonunda olduğu gibi bir SQL sorgusu oluşturur fakat
sorguyu çalıştırmayıp string veritipinde döndürür.

Örnek::

	$sql = $this->db->get_compiled_select('tablom');
	echo $sql;
	
	// Sonuç: SELECT * FROM tablom

İkinci değer 	...
The second parameter enables you to set whether or not the active record query 
will be reset (by default it will be&mdash;just like `$this->db->get()`)::

	echo $this->db->limit(10,20)->get_compiled_select('mytable', FALSE);
	// Produces string: SELECT * FROM mytable LIMIT 20, 10 
	// (in MySQL. Other databases have slightly different syntax)
	
	echo $this->db->select('title, content, date')->get_compiled_select();

	// Produces string: SELECT title, content, date FROM mytable
	
The key thing to notice in the above example is that the second query did not 
utilize `$this->db->from()`_ and did not pass a table name into the first 
parameter. The reason for this outcome is because the query has not been 
executed using `$this->db->get()`_ which resets values or reset directly 
using `$this-db->reset_query()`_.


$this->db->get_where()
======================

Identical to the above function except that it permits you to add a
"where" clause in the second parameter, instead of using the db->where()
function::

	$query = $this->db->get_where('mytable', array('id' => $id), $limit, $offset);

Please read the about the where function below for more information.

.. note:: get_where() was formerly known as getwhere(), which has been
removed

$this->db->select()
===================

Permits you to write the SELECT portion of your query::

	$this->db->select('title, content, date');
	$query = $this->db->get('mytable');  // Produces: SELECT title, content, date FROM mytable


.. note:: If you are selecting all (\*) from a table you do not need to
	use this function. When omitted, CodeIgniter assumes you wish to SELECT *

$this->db->select() accepts an optional second parameter. If you set it
to FALSE, CodeIgniter will not try to protect your field or table names
with backticks. This is useful if you need a compound select statement.

::

	$this->db->select('(SELECT SUM(payments.amount) FROM payments WHERE payments.invoice_id=4') AS amount_paid', FALSE); 
	$query = $this->db->get('mytable');


$this->db->select_max()
=======================

Writes a "SELECT MAX(field)" portion for your query. You can optionally
include a second parameter to rename the resulting field.

::

	$this->db->select_max('age');
	$query = $this->db->get('members');  // Produces: SELECT MAX(age) as age FROM members
	
	$this->db->select_max('age', 'member_age');
	$query = $this->db->get('members'); // Produces: SELECT MAX(age) as member_age FROM members


$this->db->select_min()
=======================

Writes a "SELECT MIN(field)" portion for your query. As with
select_max(), You can optionally include a second parameter to rename
the resulting field.

::

	$this->db->select_min('age');
	$query = $this->db->get('members'); // Produces: SELECT MIN(age) as age FROM members


$this->db->select_avg()
=======================

Writes a "SELECT AVG(field)" portion for your query. As with
select_max(), You can optionally include a second parameter to rename
the resulting field.

::

	$this->db->select_avg('age');
	$query = $this->db->get('members'); // Produces: SELECT AVG(age) as age FROM members


$this->db->select_sum()
=======================

Writes a "SELECT SUM(field)" portion for your query. As with
select_max(), You can optionally include a second parameter to rename
the resulting field.

::

	$this->db->select_sum('age');
	$query = $this->db->get('members'); // Produces: SELECT SUM(age) as age FROM members


$this->db->from()
=================

Permits you to write the FROM portion of your query::

	$this->db->select('title, content, date');
	$this->db->from('mytable');
	$query = $this->db->get();  // Produces: SELECT title, content, date FROM mytable

.. note:: As shown earlier, the FROM portion of your query can be specified
	in the $this->db->get() function, so use whichever method you prefer.

$this->db->join()
=================

Permits you to write the JOIN portion of your query::

	$this->db->select('*');
	$this->db->from('blogs');
	$this->db->join('comments', 'comments.id = blogs.id');
	$query = $this->db->get();
	
	// Produces:
	// SELECT * FROM blogs JOIN comments ON comments.id = blogs.id

Multiple function calls can be made if you need several joins in one
query.

If you need a specific type of JOIN you can specify it via the third
parameter of the function. Options are: left, right, outer, inner, left
outer, and right outer.

::

	$this->db->join('comments', 'comments.id = blogs.id', 'left');
	// Produces: LEFT JOIN comments ON comments.id = blogs.id

$this->db->where()
==================

This function enables you to set **WHERE** clauses using one of four
methods:

.. note:: All values passed to this function are escaped automatically,
	producing safer queries.

#. **Simple key/value method:**

	::

		$this->db->where('name', $name); // Produces: WHERE name = 'Joe' 

	Notice that the equal sign is added for you.

	If you use multiple function calls they will be chained together with
	AND between them:

	::

		$this->db->where('name', $name);
		$this->db->where('title', $title);
		$this->db->where('status', $status);
		// WHERE name = 'Joe' AND title = 'boss' AND status = 'active'  

#. **Custom key/value method:**
	You can include an operator in the first parameter in order to
	control the comparison:

	::

		$this->db->where('name !=', $name);
		$this->db->where('id <', $id); // Produces: WHERE name != 'Joe' AND id < 45    

#. **Associative array method:**

	::

		$array = array('name' => $name, 'title' => $title, 'status' => $status);
		$this->db->where($array);
		// Produces: WHERE name = 'Joe' AND title = 'boss' AND status = 'active'    

	You can include your own operators using this method as well:

	::

		$array = array('name !=' => $name, 'id <' => $id, 'date >' => $date);
		$this->db->where($array);

#. **Custom string:**
	You can write your own clauses manually::

		$where = "name='Joe' AND status='boss' OR status='active'";
		$this->db->where($where);


$this->db->where() accepts an optional third parameter. If you set it to
FALSE, CodeIgniter will not try to protect your field or table names
with backticks.

::

	$this->db->where('MATCH (field) AGAINST ("value")', NULL, FALSE);


$this->db->or_where()
=====================

This function is identical to the one above, except that multiple
instances are joined by OR::

	$this->db->where('name !=', $name);
	$this->db->or_where('id >', $id);  // Produces: WHERE name != 'Joe' OR id > 50

.. note:: or_where() was formerly known as orwhere(), which has been
	removed.

$this->db->where_in()
=====================

Generates a WHERE field IN ('item', 'item') SQL query joined with AND if
appropriate

::

	$names = array('Frank', 'Todd', 'James');
	$this->db->where_in('username', $names);
	// Produces: WHERE username IN ('Frank', 'Todd', 'James')


$this->db->or_where_in()
========================

Generates a WHERE field IN ('item', 'item') SQL query joined with OR if
appropriate

::

	$names = array('Frank', 'Todd', 'James');
	$this->db->or_where_in('username', $names);
	// Produces: OR username IN ('Frank', 'Todd', 'James')


$this->db->where_not_in()
=========================

Generates a WHERE field NOT IN ('item', 'item') SQL query joined with
AND if appropriate

::

	$names = array('Frank', 'Todd', 'James');
	$this->db->where_not_in('username', $names);
	// Produces: WHERE username NOT IN ('Frank', 'Todd', 'James')


$this->db->or_where_not_in()
============================

Generates a WHERE field NOT IN ('item', 'item') SQL query joined with OR
if appropriate

::

	$names = array('Frank', 'Todd', 'James');
	$this->db->or_where_not_in('username', $names);
	// Produces: OR username NOT IN ('Frank', 'Todd', 'James')


$this->db->like()
=================

This function enables you to generate **LIKE** clauses, useful for doing
searches.

.. note:: All values passed to this function are escaped automatically.

#. **Simple key/value method:**

	::

		$this->db->like('title', 'match');     // Produces: WHERE title LIKE '%match%' 

	If you use multiple function calls they will be chained together with
	AND between them::

		$this->db->like('title', 'match');
		$this->db->like('body', 'match');
		// WHERE title LIKE '%match%' AND  body LIKE '%match%

	If you want to control where the wildcard (%) is placed, you can use
	an optional third argument. Your options are 'before', 'after' and
	'both' (which is the default).

	::

		$this->db->like('title', 'match', 'before');	// Produces: WHERE title LIKE '%match'
		$this->db->like('title', 'match', 'after');		// Produces: WHERE title LIKE 'match%'
		$this->db->like('title', 'match', 'both');		// Produces: WHERE title LIKE '%match%' 

#. **Associative array method:**

	::

		$array = array('title' => $match, 'page1' => $match, 'page2' => $match);
		$this->db->like($array);
		// WHERE title LIKE '%match%' AND  page1 LIKE '%match%' AND  page2 LIKE '%match%'


$this->db->or_like()
====================

This function is identical to the one above, except that multiple
instances are joined by OR::

	$this->db->like('title', 'match'); $this->db->or_like('body', $match);
	// WHERE title LIKE '%match%' OR  body LIKE '%match%'

.. note:: or_like() was formerly known as orlike(), which has been removed.

$this->db->not_like()
=====================

This function is identical to **like()**, except that it generates NOT
LIKE statements::

	$this->db->not_like('title', 'match');  // WHERE title NOT LIKE '%match%

$this->db->or_not_like()
========================

This function is identical to **not_like()**, except that multiple
instances are joined by OR::

	$this->db->like('title', 'match');
	$this->db->or_not_like('body', 'match');
	// WHERE title  LIKE '%match% OR body NOT LIKE '%match%'

$this->db->group_by()
=====================

Permits you to write the GROUP BY portion of your query::

	$this->db->group_by("title"); // Produces: GROUP BY title

You can also pass an array of multiple values as well::

	$this->db->group_by(array("title", "date"));  // Produces: GROUP BY title, date

.. note:: group_by() was formerly known as groupby(), which has been
	removed.

$this->db->distinct()
=====================

Adds the "DISTINCT" keyword to a query

::

	$this->db->distinct();
	$this->db->get('table'); // Produces: SELECT DISTINCT * FROM table


$this->db->having()
===================

Permits you to write the HAVING portion of your query. There are 2
possible syntaxes, 1 argument or 2::

	$this->db->having('user_id = 45');  // Produces: HAVING user_id = 45
	$this->db->having('user_id',  45);  // Produces: HAVING user_id = 45 

You can also pass an array of multiple values as well::

	$this->db->having(array('title =' => 'My Title', 'id <' => $id));
	// Produces: HAVING title = 'My Title', id < 45


If you are using a database that CodeIgniter escapes queries for, you
can prevent escaping content by passing an optional third argument, and
setting it to FALSE.

::

	$this->db->having('user_id',  45);  // Produces: HAVING `user_id` = 45 in some databases such as MySQL
	$this->db->having('user_id',  45, FALSE);  // Produces: HAVING user_id = 45


$this->db->or_having()
======================

Identical to having(), only separates multiple clauses with "OR".

$this->db->order_by()
=====================

Lets you set an ORDER BY clause. The first parameter contains the name
of the column you would like to order by. The second parameter lets you
set the direction of the result. Options are asc or desc, or random.

::

	$this->db->order_by("title", "desc");  // Produces: ORDER BY title DESC

You can also pass your own string in the first parameter::

	$this->db->order_by('title desc, name asc');  // Produces: ORDER BY title DESC, name ASC

Or multiple function calls can be made if you need multiple fields.

::

	$this->db->order_by("title", "desc");
	$this->db->order_by("name", "asc"); // Produces: ORDER BY title DESC, name ASC     


.. note:: order_by() was formerly known as orderby(), which has been
	removed.

.. note:: random ordering is not currently supported in Oracle or MSSQL
	drivers. These will default to 'ASC'.

$this->db->limit()
==================

Lets you limit the number of rows you would like returned by the query::

	$this->db->limit(10);  // Produces: LIMIT 10

The second parameter lets you set a result offset.

::

	$this->db->limit(10, 20);  // Produces: LIMIT 20, 10 (in MySQL.  Other databases have slightly different syntax)

$this->db->count_all_results()
==============================

Permits you to determine the number of rows in a particular Active
Record query. Queries will accept Active Record restrictors such as
where(), or_where(), like(), or_like(), etc. Example::

	echo $this->db->count_all_results('my_table');  // Produces an integer, like 25
	$this->db->like('title', 'match');
	$this->db->from('my_table');
	echo $this->db->count_all_results(); // Produces an integer, like 17 

$this->db->count_all()
======================

Permits you to determine the number of rows in a particular table.
Submit the table name in the first parameter. Example::

	echo $this->db->count_all('my_table');  // Produces an integer, like 25

**************
Inserting Data
**************

$this->db->insert()
===================

Generates an insert string based on the data you supply, and runs the
query. You can either pass an **array** or an **object** to the
function. Here is an example using an array::

	$data = array(
		'title' => 'My title',
		'name' => 'My Name',
		'date' => 'My date'
	);
	
	$this->db->insert('mytable', $data);
	// Produces: INSERT INTO mytable (title, name, date) VALUES ('My title', 'My name', 'My date')

The first parameter will contain the table name, the second is an
associative array of values.

Here is an example using an object::

	/*
	class Myclass {
		var  $title = 'My Title';
		var  $content = 'My Content';
		var  $date = 'My Date';
	}
	*/
	
	$object = new Myclass;
	$this->db->insert('mytable', $object);
	// Produces: INSERT INTO mytable (title, content, date) VALUES ('My Title', 'My Content', 'My Date')

The first parameter will contain the table name, the second is an
object.

.. note:: All values are escaped automatically producing safer queries.

$this->db->get_compiled_insert()
================================
Compiles the insertion query just like `$this->db->insert()`_ but does not 
*run* the query. This method simply returns the SQL query as a string.

Example::

	$data = array(
		'title' => 'My title',
		'name'  => 'My Name',
		'date'  => 'My date'
	);
	
	$sql = $this->db->set($data)->get_compiled_insert('mytable');
	echo $sql;
	
	// Produces string: INSERT INTO mytable (title, name, date) VALUES ('My title', 'My name', 'My date')

The second parameter enables you to set whether or not the active record query 
will be reset (by default it will be--just like `$this->db->insert()`_)::
	
	echo $this->db->set('title', 'My Title')->get_compiled_insert('mytable', FALSE);
	
	// Produces string: INSERT INTO mytable (title) VALUES ('My Title')
	
	echo $this->db->set('content', 'My Content')->get_compiled_insert();

	// Produces string: INSERT INTO mytable (title, content) VALUES ('My Title', 'My Content')
	
The key thing to notice in the above example is that the second query did not 
utlize `$this->db->from()`_ nor did it pass a table name into the first 
parameter. The reason this worked is because the query has not been executed 
using `$this->db->insert()`_ which resets values or reset directly using 
`$this->db->reset_query()`_.

$this->db->insert_batch()
=========================

Generates an insert string based on the data you supply, and runs the
query. You can either pass an **array** or an **object** to the
function. Here is an example using an array::

	$data = array(
		array(
			'title' => 'My title',
			'name' => 'My Name',
			'date' => 'My date'
		),
		array(
			'title' => 'Another title',
			'name' => 'Another Name',
			'date' => 'Another date'
		)
	);
	
	$this->db->insert_batch('mytable', $data);
	// Produces: INSERT INTO mytable (title, name, date) VALUES ('My title', 'My name', 'My date'),  ('Another title', 'Another name', 'Another date')

The first parameter will contain the table name, the second is an
associative array of values.

.. note:: All values are escaped automatically producing safer queries.

$this->db->set()
================

This function enables you to set values for inserts or updates.

**It can be used instead of passing a data array directly to the insert
or update functions:**

::

	$this->db->set('name', $name);
	$this->db->insert('mytable');  // Produces: INSERT INTO mytable (name) VALUES ('{$name}')

If you use multiple function called they will be assembled properly
based on whether you are doing an insert or an update::

	$this->db->set('name', $name);
	$this->db->set('title', $title);
	$this->db->set('status', $status);
	$this->db->insert('mytable'); 

**set()** will also accept an optional third parameter ($escape), that
will prevent data from being escaped if set to FALSE. To illustrate the
difference, here is set() used both with and without the escape
parameter.

::

	$this->db->set('field', 'field+1', FALSE);
	$this->db->insert('mytable'); // gives INSERT INTO mytable (field) VALUES (field+1)
	$this->db->set('field', 'field+1');
	$this->db->insert('mytable'); // gives INSERT INTO mytable (field) VALUES ('field+1')


You can also pass an associative array to this function::

	$array = array(
		'name' => $name,
		'title' => $title,
		'status' => $status
	);
	
	$this->db->set($array);
	$this->db->insert('mytable');

Or an object::

	/*
	class Myclass {
		var  $title = 'My Title';
		var  $content = 'My Content';
		var  $date = 'My Date';
	}
	*/
	
	$object = new Myclass;
	$this->db->set($object);
	$this->db->insert('mytable');


*************
Updating Data
*************

$this->db->update()
===================

Generates an update string and runs the query based on the data you
supply. You can pass an **array** or an **object** to the function. Here
is an example using an array::

	$data = array(
		'title' => $title,
		'name' => $name,
		'date' => $date
	);
	
	$this->db->where('id', $id);
	$this->db->update('mytable', $data);
	// Produces: // UPDATE mytable  // SET title = '{$title}', name = '{$name}', date = '{$date}' // WHERE id = $id

Or you can supply an object::

	/*
	class Myclass {
		var  $title = 'My Title';
		var  $content = 'My Content';
		var  $date = 'My Date';
	}
	*/
	
	$object = new Myclass;
	$this->db->where('id', $id);
	$this->db->update('mytable', $object);
	// Produces: // UPDATE mytable  // SET title = '{$title}', name = '{$name}', date = '{$date}' // WHERE id = $id

.. note:: All values are escaped automatically producing safer queries.

You'll notice the use of the $this->db->where() function, enabling you
to set the WHERE clause. You can optionally pass this information
directly into the update function as a string::

	$this->db->update('mytable', $data, "id = 4");

Or as an array::

	$this->db->update('mytable', $data, array('id' => $id));

You may also use the $this->db->set() function described above when
performing updates.

$this->db->update_batch()
=========================

Generates an update string based on the data you supply, and runs the query.
You can either pass an **array** or an **object** to the function.
Here is an example using an array::

	$data = array(
	   array(
	      'title' => 'My title' ,
	      'name' => 'My Name 2' ,
	      'date' => 'My date 2'
	   ),
	   array(
	      'title' => 'Another title' ,
	      'name' => 'Another Name 2' ,
	      'date' => 'Another date 2'
	   )
	);

	$this->db->update_batch('mytable', $data, 'title'); 

	// Produces: 
	// UPDATE `mytable` SET `name` = CASE
	// WHEN `title` = 'My title' THEN 'My Name 2'
	// WHEN `title` = 'Another title' THEN 'Another Name 2'
	// ELSE `name` END,
	// `date` = CASE 
	// WHEN `title` = 'My title' THEN 'My date 2'
	// WHEN `title` = 'Another title' THEN 'Another date 2'
	// ELSE `date` END
	// WHERE `title` IN ('My title','Another title')

The first parameter will contain the table name, the second is an associative
array of values, the third parameter is the where key.

.. note:: All values are escaped automatically producing safer queries.

$this->db->get_compiled_update()
================================

This works exactly the same way as ``$this->db->get_compiled_insert()`` except
that it produces an UPDATE SQL string instead of an INSERT SQL string.

For more information view documentation for `$this->get_compiled_insert()`_.


*************
Deleting Data
*************

$this->db->delete()
===================

Generates a delete SQL string and runs the query.

::

	$this->db->delete('mytable', array('id' => $id));  // Produces: // DELETE FROM mytable  // WHERE id = $id

The first parameter is the table name, the second is the where clause.
You can also use the where() or or_where() functions instead of passing
the data to the second parameter of the function::

	$this->db->where('id', $id);
	$this->db->delete('mytable');
	
	// Produces:
	// DELETE FROM mytable
	// WHERE id = $id


An array of table names can be passed into delete() if you would like to
delete data from more than 1 table.

::

	$tables = array('table1', 'table2', 'table3');
	$this->db->where('id', '5');
	$this->db->delete($tables);


If you want to delete all data from a table, you can use the truncate()
function, or empty_table().

$this->db->empty_table()
========================

Generates a delete SQL string and runs the
query.::

	  $this->db->empty_table('mytable'); // Produces: DELETE FROM mytable


$this->db->truncate()
=====================

Generates a truncate SQL string and runs the query.

::

	$this->db->from('mytable');
	$this->db->truncate();
	
	// or  
	
	$this->db->truncate('mytable');
	
	// Produce:
	// TRUNCATE mytable 

.. note:: If the TRUNCATE command isn't available, truncate() will
	execute as "DELETE FROM table".
	
$this->db->get_compiled_delete()
================================
This works exactly the same way as ``$this->db->get_compiled_insert()`` except
that it produces a DELETE SQL string instead of an INSERT SQL string.

For more information view documentation for `$this->get_compiled_insert()`_.

***************
Method Chaining
***************

Method chaining allows you to simplify your syntax by connecting
multiple functions. Consider this example::

	$query = $this->db->select('title')
				->where('id', $id)
				->limit(10, 20)
				->get('mytable');

.. _ar-caching:

*********************
Active Record Caching
*********************

While not "true" caching, Active Record enables you to save (or "cache")
certain parts of your queries for reuse at a later point in your
script's execution. Normally, when an Active Record call is completed,
all stored information is reset for the next call. With caching, you can
prevent this reset, and reuse information easily.

Cached calls are cumulative. If you make 2 cached select() calls, and
then 2 uncached select() calls, this will result in 4 select() calls.
There are three Caching functions available:

$this->db->start_cache()
========================

This function must be called to begin caching. All Active Record queries
of the correct type (see below for supported queries) are stored for
later use.

$this->db->stop_cache()
=======================

This function can be called to stop caching.

$this->db->flush_cache()
========================

This function deletes all items from the Active Record cache.

Here's a usage example::

	$this->db->start_cache();
	$this->db->select('field1');
	$this->db->stop_cache();
	$this->db->get('tablename');
	//Generates: SELECT `field1` FROM (`tablename`)
	
	$this->db->select('field2');
	$this->db->get('tablename');
	//Generates:  SELECT `field1`, `field2` FROM (`tablename`)
	
	$this->db->flush_cache();
	$this->db->select('field2');
	$this->db->get('tablename');
	//Generates:  SELECT `field2` FROM (`tablename`)


.. note:: The following statements can be cached: select, from, join,
	where, like, group_by, having, order_by, set



*******************
Reset Active Record
*******************

Resetting Active Record allows you to start fresh with your query without 
executing it first using a method like $this->db->get() or $this->db->insert(). 
Just like the methods that execute a query, this will *not* reset items you've 
cached using `Active Record Caching`_.

This is useful in situations where you are using Active Record to generate SQL 
(ex. ``$this->db->get_compiled_select()``) but then choose to, for instance, 
run the query::

	// Note that the second parameter of the get_compiled_select method is FALSE
	$sql = $this->db->select(array('field1','field2'))
					->where('field3',5)
					->get_compiled_select('mytable', FALSE);

	// ...
	// Do something crazy with the SQL code... like add it to a cron script for
	// later execution or something...
	// ...

	$data = $this->db->get()->result_array();

	// Would execute and return an array of results of the following query:
	// SELECT field1, field1 from mytable where field3 = 5;
