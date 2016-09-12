---
title: Meu primeiro CRUD com CodeIgniter [parte 5]
layout: post
categories: codeigniter
image: "img/img-codeigniter.png"
descripton: "Criando o Deletar do CRUD"
---


Bom, já temos o cadastrar, listar e atualizar do nosso crud e para finalizar criaremos o apagar que segue o mesmo padrão. 
Na view `DELETE` carregamos o formulário com os campos nome, email e login desabilitados e um botão para excluir o registro. Utilizando a mesma lógica do update chamando a função get_byid que traz o registro referente ao id da url.

`application/views/telas/delete.php:`

```php
<div class="row">
	<div class="col-md-6 col-md-offset-3">
		<?php 
		echo '<div class="well">';
		echo '<h1>Apagar Usuários</h1>';
		echo '</div>';
		$iduser = $this->uri->segment(3);
		if ($iduser==NULL) redirect ('crud/retrieve');
		$query = $this->crud->get_byid($iduser)->row();
		echo form_open("crud/delete/$iduser");
		echo '<div class="form-group">';
		echo form_label('Nome completo: ');
		echo form_input(array('name' => 'nome','class' => 'form-control'), set_value('nome',$query->nome),'disabled="disabled"');
		echo form_label('Email: ');
		echo form_input(array('name' => 'email','class' => 'form-control'), set_value('email', $query->email), 'disabled="disabled"');
		echo form_label('Login: ');
		echo form_input(array('name' => 'login','class' => 'form-control'), set_value('login',$query->login), 'disabled="disabled"');
		echo '</div>';
		echo form_submit(array('name' => 'excluir','class' => 'btn btn-primary'), 'Excluir Registro');
		echo form_hidden('idusuario',$query->id); //campo oculto com id de usuario para usar na condição do controller
		echo form_close();
		?>
	</div>
</div>
```

O controller `CRUD` verifica se foi passado algum id pelo post do formulário e chama a função `do_delete` no model passando a condição.

`application/controller/crud.php:`

```php
<?php
	...
	//A VIEW DELETE, VALIDA SE FOI ENVIADO VIA POST ALGUM ID E CASO TRUE ACESSA A FUNCTION 'DO_DELETE' NO MODEL E APAGA O REGISTRO REFERENTE AO ID.
	public function delete(){
		if ($this->input->post('idusuario')>0):
			$this->crud->do_delete(array('id'=>$this->input->post('idusuario')));
		endif;

		$dados = array(
			'titulo' => 'CRUD &raquo; Delete',
			'tela' => 'delete',
		);
		$this->load->view('crud',$dados);
	}
	...
?>
```


E o model `CRUD_MODEL` através da função `do_delete` apaga o registro e redireciona para a página com a lista de registro.

`application/model/crud_model.php:`

```php
<?php
	...
	// FUNÇÃO RESPONSAVEL POR DELETAR O REGISTRO NA BASE
	public function do_delete($condicao=NULL){
			if($condicao!=NULL):
				$this->db->delete('usuario_table',$condicao);
			$this->session->set_flashdata('excluirok', '<div class="alert alert-success" role="alert">Registro deletado com sucesso</div>');
			redirect('crud/retrieve');
			endif;
	}
	...
?>
```




Nosso CRUD está pronto com todas as funções básicas, finalizamos por aqui e caso queira ver os outros post da sequência segue abaixo:

- [Estrutura e configurações necessárias - Parte1 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part1 %}){:class="links_post"} <br>
- [Cadastrar (CRUD) - Parte2 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part2 %}){:class="links_post"} <br>
- [Atualizar (CRUD) - Parte4 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part4 %}){:class="links_post"} <br>
- [Apagar (CRUD) - Parte5 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part5 %}){:class="links_post"}

Caso queira assistir os videos onde ensina passo-a-passo tudo que tentei exemplificar nos posts acesse a playlist da [RBTech](https://www.youtube.com/watch?v=1XnfWac0U14&list=PLInBAd9OZCzz2vtRFDwum0OyUmJg8UqDV){:target="_blank"}.