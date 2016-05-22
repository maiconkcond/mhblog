---
title: Meu primeiro CRUD com CodeIgniter [parte 2]
layout: post
categories: codeigniter
image: "img/ci_logo_flame.png"
descripton: "Criando o Cadastro do CRUD"
---


No [post anterior]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part1 %}){:class="links_post"}{:target="_blank"}  realizamos a parte de configuração e a estrutura básica para criação do nosso CRUD, agora iremos começar a cria-lo pelo CREATE.

A view `CREATE` irá conter um formulário no qual será utilizado  o helper form do codeigniter. Também terá uma verificação no caso de dar erro na validação do form, o validation_erros irá mostrar a mensagem de erro ou caso contrario uma mensagem de sucesso.

`application/view/telas/create.php:`

```php
<div class="row">
<div class="col-md-6 col-md-offset-3">
	<?php
	echo '<div class="well">';
	echo '<h1>Cadastro de Usuários</h1>';
	echo '</div>';
	echo form_open('crud/create');
	echo validation_errors('<div class="alert alert-danger" role="alert">','</div>');
	if ($this->session->flashdata('cadastrook')):
	echo '<div class="alert alert-success" role="alert">'.$this->session->flashdata('cadastrook').'</div>';
	endif;
	echo '<div class="form-group">';
	echo form_label('Nome completo: ');
	echo form_input(array('name' => 'nome','class' => 'form-control', 'placeholder' => 'Seu nome'), set_value('nome'),'autofocus');
	echo form_label('Email: ');
	echo form_input(array('name' => 'email','class' => 'form-control', 'placeholder' => 'Seu email'), set_value('email'));
	echo form_label('Login: ');
	echo form_input(array('name' => 'login','class' => 'form-control', 'placeholder' => 'Seu login'), set_value('login'));
	echo form_label('Senha: ');
	echo form_password(array('name' => 'senha','class' => 'form-control', 'placeholder' => 'Sua senha'), set_value('senha'));
	echo form_label('Repita-senha: ');
	echo form_password(array('name' => 'senha2','class' => 'form-control', 'placeholder' => 'Sua senha novamente'), set_value('senha2'));
	echo '</div>';
	echo form_submit(array('name' => 'cadastrar','class' => 'btn btn-primary'), 'Cadastrar');
	echo form_close();
	?>
	</div>
</div>
```

No controller `CRUD` iremos primeiro declarar os contrutores que irão ser utilizados ao longo do projeto e criaremos também a função create, conforme código abaixo:

`application/controller/crud.php:`

```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');
	class Crud extends CI_Controller {

		// A FUNÇÃO CARREGA AS LIBRARY,HELPES E O MODEL
		public function __construct(){
			parent:: __construct();
			$this->load->helper('url');
			$this->load->helper('form');
			$this->load->library('form_validation');
			$this->load->helper('array');
			$this->load->model('crud_model','crud');
			$this->load->library('session');
			$this->load->library('table');
		}
		...

		//VIEW CREATE, REALIZA A VALIDAÇÃO DO FORMULARIO, PEGANDO DADOS E INSERINDO NO BANCO ATRAVES DA FUNCTION 'DO_INSERT' QUE ESTÁ NO MODEL
		public function create(){
			$this->form_validation->set_rules('nome', 'NOME', 'trim|required|max_length[50]|ucwords');
			$this->form_validation->set_message('is_unique','Este %s cadastrado no sistema');
			$this->form_validation->set_rules('email', 'EMAIL', 'trim|required|max_length[50]|strtolower|valid_email, is_unique[usuario_table.email]');
			$this->form_validation->set_rules('login', 'LOGIN', 'trim|required|max_length[25]|strtolower,is_unique[usuario_table.login]');
			$this->form_validation->set_rules('senha', 'SENHA', 'trim|required|strtolower');
			$this->form_validation->set_message('matches','O campo %s está diferente do campo %s');
			$this->form_validation->set_rules('senha2', 'REPITA SENHA', 'trim|required|strtolower|matches[senha]');

			if($this->form_validation->run()==TRUE):
				$dados = elements(array('nome','email','login','senha'),$this->input->post());
				$dados['senha'] = md5($dados['senha']);
				$this->crud->do_insert($dados);
			endif;	

			$dados = array(
				'titulo' => 'CRUD &raquo; Create',
				'tela' => 'create',
			);
			$this->load->view('crud',$dados);
		}	
	}
```

O que fizemos na function `CREATE` foi usar a library `form_validation` para fazer a validação no formulário e se a validação estiver OK os dados é armazenado no array e a senha encriptografada, o registro é enviado para a function `do_insert` localizada no MODEL que é o responsável por salvar o registro.	

Já no model `CRUD_MODEL` será validado se a variável com os dados vieram nulo e caso contrario o insere no banco e envia a mensagem de sucesso para a view e faz o redirecionamento para a mesma página.

`application/model/crud_model.php:`

```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');
	class Crud_model extends CI_Model{	
		// FUNÇÃO RESPONSAVEL POR INSERIR O REGISTRO NA BASE
		public function do_insert($dados=NULL){
			if ($dados!=NULL):
				$this->db->insert('usuario_table',$dados);	
				$this->session->set_flashdata('cadastrook', 'Cadastro efetuado com sucesso');
				redirect('crud/create');
			endif;
		}	
	}
```

(OBS: Não entrei em detalhes de banco de dados, porém para que funcione você deve ter a tabela `usuario_table` criada com os campos nome, login, email e senha)

Até esse ponto já temos o cadastro funcionando, fazendo as devidas validações e inserindo no banco. Por hoje ficamos por aqui e no próximo post será mostrado o `listar` que você pode acessar abaixo:

- [Estrutura e configurações necessárias - Parte1 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part1 %}){:class="links_post"} <br>
- [Lista (CRUD) - Parte3 ](#){:class="links_post"} <br>
- [Atualizar (CRUD) - Parte4 ](#){:class="links_post"} <br>
- [Apagar (CRUD) - Parte5 ](#){:class="links_post"}

Lembrando que para melhor entendimento vale muito a pena ver os videos no [RBTech](https://www.youtube.com/watch?v=1XnfWac0U14&list=PLInBAd9OZCzz2vtRFDwum0OyUmJg8UqDV){:target="_blank"}.