---
layout: post
title:  "Adicionando chave RSA no GitHub"
date:   2015-06-11 08:50:01
categories: ssh rsa github dev
---

Procedimento para adicionar chave RSA no GitHub.

Abra um terminal (com usuário que irá commitar alterações) e digite o código abaixo substituindo o e-mail de exemplo pelo seu e-mail cadastrado no GitHub.

	$ ssh-keygen -t rsa -b 4096 -C "seu_email@example.com.br"

Verifique se o endereço é a pasta .ssh na home do seu usuário e tecle enter na tela abaixo caso não queira alterar

	# Enter file in which to save the key (/Users/seuusuario/.ssh/id_rsa): [Press enter]

Aperte Enter duas vezes sem digitar nada caso não queira utilizar uma frase de segurança

	# Enter passphrase (empty for no passphrase): [Type a passphrase]
	# Enter same passphrase again: [Type passphrase again]

A chave será criada com uma confirmação próxima da abaixo:

	Your identification has been saved in /Users/you/.ssh/id_rsa.
	# Your public key has been saved in /Users/you/.ssh/id_rsa.pub.
	# The key fingerprint is:
	# 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db seu_email@example.com.br

Veja a sua chave pública para poder a copiar:

	$ cd ~/.ssh
	$ cat id_rsa.pub

Selecione a string que inicia com ssh-rsa e termina com seu e-mail e copie (Ctrl+Shift+C ou botão direito e copiar).

Logado na página do GitHub, logue e vá em Configurações (Settings) > SSH Keys ou diretamente via [url direta](https://github.com/settings/ssh "url direta para acesso à adição de SSH no GitHub").

Clique em Adicionar chave SSH (Add SSH Key) dê um nome para o computador que acessa e adicione a chave pública.

De volta ao terminal configure o e-mail e usuário a ser usado globalmente para git alterando nos comandos abaixo o nome de usuário e e-mail.

	$ git config --global user.name "nomeDeUsuario"
	$ git config --global user.email "seu_email@example.com.br"
