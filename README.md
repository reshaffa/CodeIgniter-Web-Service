# Restfull-api CodeIgniter Library
**Apa itu Restfull-api..?**
merupakan implementasi dari API *(Application Programming Interface)*. REST *(Representional State Transfer)* adalah suatu arsitektur metode komunikasi yang menggunakan protokol HTTP untuk pertukaran data dan metode ini sering diterapkan dalam pengembangan aplikasi. Dimana tujuannya adalah untuk menjadikan sistem yang memiliki performa yang baik, cepat dan mudah untuk di kembangkan *(scale)* terutama dalam pertukaran dan komunikasi data.

**Bagaimana membangun Restfull-api menggunakan CodeIgniter.?**
Ada beberapa langkah dan beberapa library yang harus anda download. Berikut ini langkah-langkahnya.
- Download CodeIgniter disini [CodeIgniter](https://www.codeigniter.com/download)
- Download Library disini [Github](https://github.com/chriskacerguis/codeigniter-restserver)
- Lakukan beberapa konfigurasi seperti berikut ini:
	- Copy file **rest.php** ke folder  /appclication/config/rest.php
	- Copy file **REST_Contoller.php** dan **Format.php** ke dalam folder /application/libraries/...
	- Muatlah sebuah file Restfull.php di folder application/controllers/ *ini membuat controllers*
	- Isikan baris kode berikut ke file Restfull.php
		```php
		<?php
			defined('BASEPATH') OR exit('No direct script access allowed');

			require(APPPATH.'/libraries/REST_Controller.php');
			require(APPPATH.'/libraries/Format.php');
			use Restserver\Libraries\REST_Controller;

			class Restfull extends REST_Controller{

				function __construct() {
			        parent::__construct();
			        $this->load->database();
			    }
			}
		

	- Buatlah sebuah model dengan nama App_model.php *ini ada di folder application/models/App_model.php*
	- Sisipkan baris model sederhana berikut ini:
		```php
		<?php
			defined('BASEPATH') OR exit('No direct script access allowed');

			class App_model extens CI_Model{

				function vrow($table,$param){
					if(empty($param)){
						$query = $this->db->get($table);
						return $query->result();
					}else{
						$cols  = $this->db->primary($table); 
						$query = $this->db->get_where($table,array($cols=>$param));
						if($query->num_rows()==1){
							return $query->row();
						}else{
							return false;
						}
					}
				}
			}


	- Buka kembali controller Restfull.php dan sisipkan baris berikut ini:
	```php
		function index_get(){
			$id=$this->get('id');
				if(empty($id)){
					$response = $this->api->vrow('nm_tabel');
		            $this->response($response, 200);
		        }
	 
	       		$exec = $this->api->vrow('nm_tabel',$id);
	         
		        if($exec){
		        	$response = array('code'=>200,'message'=>'Your request has been success','news'=>$exec);
		            $this->response($response, 200); // 200 being the HTTP response code
		        }else{
		        	$response = array('code'=>201,'message'=>'Upss, problem your request!!');
		            $this->response($response, 404);
		        }		
	    }

	- Silahkan buka browser dan ketik http://localhost/rest-full-api/restfull?id=1
	- Maka secara default akan menampilkan data dalam bentuk format file json.