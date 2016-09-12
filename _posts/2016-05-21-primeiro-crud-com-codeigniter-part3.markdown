---
title: Meu primeiro CRUD com CodeIgniter [parte 3]
layout: post
categories: codeigniter
image: "img/img-codeigniter.png"
descripton: "Criando o Listar do CRUD"
---


Voltando para nossa série de posts de como criar um CRUD com o codeigniter nesse post iremos listar os registros cadastrado, caso não tenha lido o [post anterior]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part1 %}){:class="links_post"}{:target="_blank"}  fizemos o cadastro do nosso CRUD.

Vamos começar pela view `RETRIEVE`, teremos uma condição que será utilizando quando criarmos a função delete que irá mostrar se o registro foi deletado com sucesso, mais abaixo utilizamos library `table` que criará uma tabela para listar os dados que serão trazidos do banco através do foreach será listado um registro por linha. Observe também que é criado dois links que direciona para view update e outro para view delete já pegando o id do registro correspondente a linha.

`application/views/telas/retrieve.php:`

```php
<div class="row">
	<div class="col-md-6 col-md-offset-3">
		<?php 
		echo '<div class="well">';
		echo '<h1>Lista de Usuários</h1>';
		echo '</div>';
		if ($this->session->flashdata('excluirok')):
			echo '<p>'.$this->session->flashdata('excluirok').'</p>';
		endif;
		$template = array(
			'table_open' => '<table class="table table-striped">'
		);
		$this->table->set_template($template);
		$this->table->set_heading('ID','Nome','Email','Login','Operações');
		foreach ($usuarios as $linha):
			$this->table->add_row($linha->id, $linha->nome, $linha->email,$linha->login, anchor("crud/update/$linha->id",'Editar').' - '.anchor("crud/delete/$linha->id",'Excluir'));
		endforeach;
		echo $this->table->generate();
		?>
	</div>
</div>
```

No controller `CRUD` iremos inserir a função retrieve que irá trazer os registros da tabela e carregar na view. 

`application/controller/crud.php:`

```php
<?php
	...
	//CARREGANDO A VIEW RETRIEVE E CHAMANDO A FUNCTION 'GET_ALL' QUE TRAZ TODOS OS REGISTROS QUE ESTÁ NO MODEL
	public function retrieve(){
		$dados = array(
			'titulo' => 'CRUD &raquo; Retrieve',
			'tela' => 'retrieve',
			'usuarios' => $this->crud->get_all()->result(),
			);
		$this->load->view('crud',$dados);
	}
	...
?>
```

Iremos inserir a função get_all no model `CRUD_MODEL` responsavel por retornar todos os registros da tabela e passar para nosso controlador

`application/model/crud_model.php:`

```php
<?php
	...
	// FUNÇÃO RESPONSAVEL POR PEGAR TODOS OS REGISTROS DA TABELA 'usuario_table' NA BASE
	public function get_all(){
		return $this->db->get('usuario_table');
	}
	...
?>
```

Agora nossa view já lista os registros, conforme imagem abaixo:

<img src="../img/lista.png" class="img-responsive img-thumbnail" alt="Lista de usuarios">


Ficamos por aqui e no proximo post realizaremos a criação do `update` que possibilitará realizamos atualizações nos registros. Acesse os outros posts abaixo:


- [Estrutura e configurações necessárias - Parte1 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part1 %}){:class="links_post"} <br>
- [Cadastrar (CRUD) - Parte2 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part2 %}){:class="links_post"} <br>
- [Atualizar (CRUD) - Parte4 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part4 %}){:class="links_post"} <br>
- [Apagar (CRUD) - Parte5 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part5 %}){:class="links_post"}

Lembrando que para melhor entendimento vale muito a pena ver os videos no [RBTech](https://www.youtube.com/watch?v=1XnfWac0U14&list=PLInBAd9OZCzz2vtRFDwum0OyUmJg8UqDV){:target="_blank"}.