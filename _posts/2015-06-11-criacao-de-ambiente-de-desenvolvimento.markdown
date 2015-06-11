---
layout: post
title:  "Criação de Ambiente de Desenvolvimento"
date:   2015-06-11 08:50:01
categories: ambiente dev
---

Procedimento para criação de ambiente de desenvolvimento.

Usando CentOS 6.5 ou 7

Instalar as bibliotecas conforme:

	$ sudo yum install python-devel readline-devel gcc gcc-c++ lynx wget bzip2 bzip2-devel bzip2-lib libjpeg-devel make patch openssl-devel zlib-devel epel-release bash-completion

Baixar e rodar o buildout.python

#### Dentro da pasta de projetos

	$ git clone https://github.com/collective/buildout.python.git
	$ cd buildout.python/
	$ python2.7 boostrap.py
	$ ./bin/buildout -vvvt 20

Baixar e rodar o repositório de buildout da Prodam SP Sites

#### Dentro da pasta de projetos

	$ git clone git@github.com:prodamspsites/prodam_buildout.git
	$ cd prodam_buildout/
	$ ../buildout.python/bin/virtualenv-2.7 py27
	$ source ./py27/bin/activate
	$ python2.7 boostrap.py -c development.cfg
	$ ./bin/buildout -vvvt 20 -c development.cfg

#### Subir a instância

	$ ./bin/instance fg
