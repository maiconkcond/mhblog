---
title: Meu primeiro CRUD com CodeIgniter [parte 4]
layout: post
categories: codeigniter
image: "img/img-codeigniter.png"
descripton: "Criando o Alterar do CRUD"
---


Nesse post iremos atualizar os registros da tabela, caso não tenha lido o [post anterior]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part1 %}){:class="links_post"}{:target="_blank"} fizemos o listar do nosso CRUD.

Na view `UPDATE` primeiro utilizamos a variavél `$iduser` para pegar o terceiro seguimento da url, caso seja nulo redireciona para o listar. Temos também que buscar o registro referente ao id no banco e com ele carregar os itens nos campos para alteração pelo `set_value`. Confira o código abaixo:

`application/views/telas/update.php:`

```php
<div class="row">
	<div class="col-md-6 col-md-offset-3">
		<?php 
		echo '<div class="well">';
		echo '<h1>Alteração de Usuários</h1>';
		echo '</div>';
		$iduser = $this->uri->segment(3);
		if ($iduser==NULL) redirect ('crud/retrieve');
		$query = $this->crud->get_byid($iduser)->row();
		echo form_open("crud/update/$iduser");
		echo validation_errors('<div class="alert alert-danger" role="alert">','</div>');
		if ($this->session->flashdata('edicaook')):
			echo '<div class="alert alert-success" role="alert">'.$this->session->flashdata('edicaook').'</div>';
		endif;
		echo '<div class="form-group">';
		echo form_label('Nome completo: ');
		echo form_input(array('name' => 'nome','class' => 'form-control'), set_value('nome',$query->nome),'autofocus');
		echo form_label('Email: ');
		echo form_input(array('name' => 'email','class' => 'form-control'), set_value('email', $query->email), 'disabled="disabled"');
		echo form_label('Login: ');
		echo form_input(array('name' => 'login','class' => 'form-control'), set_value('login',$query->login), 'disabled="disabled"');
		echo form_label('Senha: ');
		echo form_password(array('name' => 'senha','class' => 'form-control'), set_value('senha'));
		echo form_label('Repita-senha: ');
		echo form_password(array('name' => 'senha2','class' => 'form-control'), set_value('senha2'));
		echo '</div>';
		echo form_submit(array('name' => 'alterar','class' => 'btn btn-primary'), 'Alterar Dados');
		echo form_hidden('idusuario',$query->id); //campo oculto com id de usuario para usar na condição do controller
		echo form_close();
		?>
	</div>
</div>
```

O controller `CRUD` irá fazer validações como fizemos no create porém vamos alterar somente o nome e a senha, os outros campos ficaram disabilitados, como mostrado no código acima (disabled="disabled"). Caso a validação seja verdadeira armazena os dados na variavel e envia para o model junto com a condição.  

`application/controller/crud.php:`

```php
<?php
	...
	//CARREGANDO A VIEW UPDATE, REALIZANDO A VALIDAÇÃO DO FORMULARIO, E ATUALIZANDO NO BANCO CHAMANDO A FUNCTION 'DO_UPDATE' QUE ESTÁ NO MODEL
	public function update(){
		$this->form_validation->set_rules('nome', 'NOME', 'trim|required|max_length[50]|ucwords');
		$this->form_validation->set_rules('senha', 'SENHA', 'trim|required|strtolower');
		$this->form_validation->set_message('matches','O campo %s está diferente do campo %s');
		$this->form_validation->set_rules('senha2', 'REPITA SENHA', 'trim|required|strtolower|matches[senha]');

		if($this->form_validation->run()==TRUE):
			$dados = elements(array('nome','senha'),$this->input->post());
			$dados['senha'] = md5($dados['senha']);
			$this->crud->do_update($dados, array('id'=>$this->input->post('idusuario')));
		endif;	

		$dados = array(
			'titulo' => 'CRUD &raquo; Update',
			'tela' => 'update',
		);
		$this->load->view('crud',$dados);
	}
	...
?>
```

O model `CRUD_MODEL` terá mais duas funções, a primeira será utilizada pela view que passa o id por parâmetro para buscar o registro correspondente e o retorna.
A segunda função recebe os dados e a condição do controller e atualiza a tabela.

`application/model/crud_model.php:`

```php
<?php
	...
	// FUNÇÃO RESPONSAVEL POR IR NA BASE E PEGAR O REGISTRO REFERENTE AO ID
	public function get_byid($id=NULL){
		if ($id!=NULL):
			$this->db->where('id',$id);
			$this->db->limit(1); 
			return $this->db->get('usuario_table');
			else:
				return FALSE;
			endif;
	}

	// FUNÇÃO RESPONSAVEL POR ATUALIZAR O REGISTRO NA BASE
	public function do_update($dados=NULL,$condicao=NULL){
		if ($dados!=NULL && condicao!=NULL):
			$this->db->update('curso_ci',$dados,$condicao);	
		$this->session->set_flashdata('edicaook', 'Cadastro alterado com sucesso');
		redirect(current_url());
		endif;
	}
	...
?>
```

Finalizamos mais uma parte do nosso crud e o proximo post será o ultimo da sequência onde criaremos o apagar do crud.
Acesse os outros posts abaixo:

- [Estrutura e configurações necessárias - Parte1 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part1 %}){:class="links_post"} <br>
- [Cadastrar (CRUD) - Parte2 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part2 %}){:class="links_post"} <br>
- [Listar (CRUD) - Parte4 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part3 %}){:class="links_post"} <br>
- [Apagar (CRUD) - Parte5 ]({% post_url 2016-05-21-primeiro-crud-com-codeigniter-part5 %}){:class="links_post"}

Lembrando que para melhor entendimento vale muito a pena ver os videos no [RBTech](https://www.youtube.com/watch?v=1XnfWac0U14&list=PLInBAd9OZCzz2vtRFDwum0OyUmJg8UqDV){:target="_blank"}.