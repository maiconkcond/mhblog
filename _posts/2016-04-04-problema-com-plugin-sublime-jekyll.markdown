---
title: Problema com plugin Sublime-jekyll
layout: post
categories: [code, editors]
descripton: Dica para não perder tempo com problema bobo.
image: img/problema-com-plugin-sublime-jekyll.png
---

Após instalar o plugin `sublime-jekyll` no Sublime Text 3 através do Command Palette tive alguns problemas para utilizar. Quando tentava cria um post ou remover era apresentando um erro, conforme imagem abaixo:


![Mensagem de erro no sublime](/img/erro_sublime.png){:class="center"}

Bom, para resolver é preciso alterar as configurações do projeto inserindo os comandos do Jekyll.
Para realizar a alteração tive de criar um Project `(Project > Save Project As)`, que irá ficar responsavél de guardar as configurações do meu projeto, como pastas, configurações dos plugin(Jekyll) e etc. Dê um nome para seu Project e salve na pasta do seu projeto.
Após salvar o arquivo agora é necessario realizar as configurações, para isso você pode ver o [Documento oficial](http://sublime-jekyll.readthedocs.org/en/latest/settings.html){:target="_blank"}. Abaixo como ficou minha configuração: 

	{
	"folders":
	[
		{
			"path": "C:/Users/Nome_do_seu_usuario/Sua_pasta_projetos/Seu_projeto/"
		}
	],

    "settings":
    {
        "Jekyll":
        {
            "jekyll_posts_path": "C:/Users/Nome_do_seu_usuario/Sua_pasta_projetos/Seu_projeto/_posts",
            "jekyll_drafts_path": "C:/Users/Nome_do_seu_usuario/Sua_pasta_projetos/Seu_projeto/_drafts",
            "jekyll_uploads_path": "C:/Users/Nome_do_seu_usuario/Sua_pasta_projetos/Seu_projeto/uploads",
            "jekyll_datetime_format": "%Y-%m-%d %H:%M:%S"
        }
    }
	}


Entedendo melhor o código:


	path: Responsável por carregar a pasta do projeto.


Dentro de `settings > Jekyll` ficam as configurações para utilização do plugin do jekyll

	jekyll_posts_path: Você deve apontar para seu diretório _post.

	jekyll_drafts_path: Você deve apontar para seu diretório _drafts (quando aplicavél)

	jekyll_uploads_path: Você deve apontar para seu diretório uploads (quando aplicavél)

	jekyll_datetime_format: Você define formato da data/hora


Realizando essas configurações tenho conseguido utilizar o plugin normalmente.