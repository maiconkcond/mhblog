---
title: Meu primeiro CRUD com CodeIgniter [parte 1]
layout: post
categories: codeigniter
image: "img/ci_logo_flame.png"
descripton: "Estrutura e configurações necessárias"
---


Iniciando minha jornada de aprendizado do framework codeigniter assisti uma sequência de videos muito boa, na qual de forma simples o Ricardo do [canal RBTECH](https://www.youtube.com/watch?v=1XnfWac0U14&list=PLInBAd9OZCzz2vtRFDwum0OyUmJg8UqDV){:target="_blank"} ensinou a criar um CRUD do zero. Para quem como eu está iniciando no aprendizado vale muito assistir o curso dele. 
Irei criar uma sequência de posts onde tentarei exemplificar o aprendizado que retirei do curso.

Primeiramente vamos entender com será a estrutura dos arquivos que serão utilizados (crie esses arquivos):

```html
<!--
//CRUD_Aula
  -//application
   		-//controllers
   	 		crud.php
   		-//models
   	 		crud_model.php
   		-//views
   	 		-//includes
   	 			header.php
   	 			menu.php
   	 			footer.php
   	 		-//telas
   	 			create.php
  	 			delete.php
   	 			retrieve.php
   	 			update.php	
   	 		-//crud.php  
   		-//config
   	 	...
   - system
     ...-->
```

Antes de começar precisamos realizar algumas configurações de banco de dados, url_base, e routes conforme mostrado abaixo:

Configurando o banco de dados em `application/config/database.php`:

```php
<?php
	...
	$db['default']['hostname'] = 'localhost';
	$db['default']['username'] = 'root';
	$db['default']['password'] = 'sua senha';
	$db['default']['database'] = 'aulas'; //nome do seu banco, no meu caso 'aulas'
	$db['default']['dbdriver'] = 'mysql';
	...	
?>
```

Configurando a URL base da nossa aplicação em `application/config/config.php`:

```php
<?php
...
$config['base_url'] = 'http://localhost/CRUD_Aula/';
...
?>
```

Configurando a ROTA inicial, ou melhor dizendo o controlador default do nosso sistema em `application/config/routes.php`:

```php
<?php
...
$route['default_controller'] = "crud";
...
?>

```

Agora que já está tudo configurado já podemos começar, GO!

Na view `HEADER` temos o inicio do HTML básico com os metadados e a chamada do estilo (no caso do exemplo estou utilizando bootstrap pela CDN) e como vocês podem observar o título está recebendo uma váriavél que será entendida mais a frente no controller/crud.php.

`application/views/includes/header.php:`

```html
<!DOCTYPE html>
	<html lang="en">
		<head>
			<meta charset="utf-8" />
			<meta http-equiv="X-UA-Compatible" content="IE-edge,chrome=1" />
			<title> <?php echo $titulo  ?></title> <!-- Titulo passado através do controlador -->
			<meta name="description" content="" />
			<meta name="author" content="Maicon" />
			<meta name="viewport" content="width=device-width; initial-scale=1.0" />
			<!-- Latest compiled and minified CSS -->
			<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" integrity="sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7" crossorigin="anonymous">
			<!-- Optional theme -->
			<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css" integrity="sha384-fLW2N01lMqjakBkx3l/M9EahuwpSfeNvV63J5ezn3uZzapT0u7EYsXMjQV+0En5r" crossorigin="anonymous">
			<!-- Latest compiled and minified JavaScript -->
			<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" integrity="sha384-0mSbJDEHialfmuBBQP6A4Qrprq5OVfW37PRR3j5ELqxss1yVqOtnepnHVP9aJ7xS" crossorigin="anonymous"></script>
		</head>
		<body>
```

Nossa view `MENU` será responsavél apenas por criar uma lista com uma ancôra para as páginas do CRUD.

`application/views/includes/menu.php:`

```php
<nav class="navbar navbar-inverse ">
	<div class="container">
		<ul class="nav navbar-nav">
			<li><?php echo anchor('crud/create',"Cadastrar") ?></li>
			<li><?php echo anchor('crud/retrieve',"Listar") ?></li>
			<li><?php echo anchor('crud/update',"Atualizar") ?></li>
			<li><?php echo anchor('crud/delete',"Apagar") ?></li>
		</ul>
	</div>
</nav>
```

Já a view `FOOTER` será simples, apenas para fechar o body e o html:

`application/views/includes/footer.php:`

```php
	</body>
</html>
```

A view `CRUD` localizada na raiz de `application/views` será responsável por montar nossa página. Como podemos observar no conteúdo abaixo ela carrega as fatias header, menu, footer e a tela que será passado por parâmetro pelo controller:

`application/views/crud.php:`

```php
<?php 
	$this->load->view('includes/header'); //Carregando o conteúdo da view header.php
	$this->load->view('includes/menu'); //Carregando o conteúdo da view menu.php
	if($tela!='') $this->load->view('telas/'.$tela); //Carregando a view setada pela váriavel $tela via parâmetro no controller
	$this->load->view('includes/footer'); //Carregando o conteúdo da view footer.php
 ?>
```

Nosso controller `CRUD` será o responsável pelas funções e será nele que iremos configurar as páginas que serão carregadas através da variável $tela. No exemplo abaixo nossa index fica com o título passado pela variável e carregada no header e não chamamos nenhuma tela por equanto.

`application/controller/crud.php:`

```php
<?php
//PÁGINA INICIAL, SEMPRE SERÁ CARREGADO A VIEW 'CRUD' E NELA SERA PUXADO A VIEW COM CONTEUDO
public function index(){
	$dados = array(
		'titulo' => 'CRUD com CodeIgniter',
		'tela' => '',
		);
	$this->load->view('crud',$dados);
}
?>
```

O array $dados armazena o titulo da página e o nome da tela que será carregada (que é o nome da view). Depois de armazenado ele enviar para o view `CRUD` que redimensiona o conteúdo. 

Por hoje é só, esse é o primeiro post da sequência, acesse os outros abaixo:

- [Cadastro (CRUD) - Parte2 ]({{% post_url 2016-05-21-primeiro-crud-com-codeigniter-part2 %}}){:class="links_post"} <br>
- [Lista (CRUD) - Parte3 ](#){:class="links_post"} <br>
- [Atualizar (CRUD) - Parte4 ](#){:class="links_post"} <br>
- [Apagar (CRUD) - Parte5 ](#){:class="links_post"}

Lembrando que para melhor entendimento vale muito a pena ver os videos no [RBTech](https://www.youtube.com/watch?v=1XnfWac0U14&list=PLInBAd9OZCzz2vtRFDwum0OyUmJg8UqDV){:target="_blank"}.
