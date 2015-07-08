---
layout: post
title:  "Trabalhando com proxy"
date:   2015-07-08 10:30:01
categories: ambiente dev proxy
---

Procedimento para criação de ambientes de desenvolvimento através de proxy autenticada.

## Configuração de Proxy no CentOS 6.5


## Ajusta de navegadores e sistema

### Acessar configuração de Rede

Para acessar configuração de Rede, aperta a tecla 'super' e digite REDE na busca. Após o resultado clique em Rede 

* Proxy da rede
* Manual
* Proxy HTTP: url_da_proxy Porta: porta_da_proxy
* Proxy HTTPS: igual acima caso não exista tratamento para HTTPS
* Proxy FTP:  igual acima caso não exista tratamento para FTP
* Servidor Socks: igual acima caso não exista tratamento para Socks
* Ignorar Máquinas: localhost, 127.0.0.0/8, ::1

## Acessar navegadores

Abra o seu navegador (Firefox/Chrome) e será exibido uma caixa de usuário e senha. Preencha e clique em Ok.

Obs: verificar existência de domínio para inserção no campo de usuário:

* DOMÍNIO\usuário.


## Configuração de YUM

Abra um terminal e insira a configuração da proxy no arquivo de configuração do YUM para ter as regras de proxy aplicadas no YUM para todos os usuários:

    $ sudo gedit /etc/yum.conf

Digite a senha e será aberto o Gedit para edição do arquivo

Insira as linhas a seguir ajustando url, porta, usuário e senha

    proxy=http://URL-DA-PROXY:PORTA-DA-PROXY/
    proxy_username=USUÁRIO
    proxy_password=SENHA (COM SÍMBOLOS SEM CONVERSÃO)
    http_proxy="http://USUÁRIO:SENHA(CONVERTER SÍMBOLOS PARA UNICODE HEXA)@URL-DA-PROXY:PORTA-DA-PROXY/"
    export http_proxy="http://USUÁRIO:SENHA(CONVERTER SÍMBOLOS PARA UNICODE HEXADECIMAL - LISTA DE EXEMPLO ABAIXO)@URL-DA-PROXY:PORTA-DA-PROXY/"

#### Tabela de exemplo com conversão de símbolos para Unicode em Hexadecimal

| Símbolo  | Unicode Hexa |
| -------- | ------------ |
| @        | %40          |
| :        | %58          |
| !        | %33          |
| #        | %35          |
| $        | %36          |

Salvar e testar o yum com atualização

    $ sudo yum update

Digite a senha e aceite as atualizações.


## Configuração do terminal

Abra um terminal e configure a proxy para uso via terminal (bash):


    $ http_proxy="http://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ HTTP_PROXY="http://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ https_proxy="http://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ HTTPS_PROXY="http://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ ftp_proxy="http://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ FTP_PROXY="http://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ all_proxy="socks://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ ALL_PROXY="socks://USUÁRIO:SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE@URL-DA-PROXY:PORTA-DA-PROXY/"
    $ no_proxy=localhost,127.0.0.0/8,::1
    $ NO_PROXY=localhost,127.0.0.0/8,::1

    $ proxy_username=USUÁRIO
    $ proxy_password=SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE
    $ proxy=http://URL-DA-PROXY:PORTA-DA-PROXY/


Ainda no terminal efetue export dessas configurações para mantê-las:

    $ export http_proxy
    $ export HTTP_PROXY
    $ export https_proxy
    $ export HTTPS_PROXY
    $ export ftp_proxy
    $ export FTP_PROXY
    $ export all_proxy
    $ export ALL_PROXY
    $ export no_proxy
    $ export NO_PROXY
    $ export proxy_username=USUÁRIO
    $ export proxy_password=SENHA-COM-CONVERSAO-DE-SÍMBOLOS-PARA-UNICODE
    $ export proxy=http://URL-DA-PROXY:PORTA-DA-PROXY/

Mande recarregar as configurações do bash:

    $ source ~/.bashrc

Verifique as configurações atuais:

    $ env | grep -i proxy

Deverá ser impresso as configurações de proxy conforme inseridas anteriormente


## Instalação de bibliotecas

Este passo é mencionado no item [Criação de Ambiente de Desenvolvimento](/ambiente/dev/2015/06/11/criacao-de-ambiente-de-desenvolvimento.html "Criação de Ambiente de Desenvolvimento").


Instalar as bibliotecas conforme:

    $ sudo yum install python-devel readline-devel gcc gcc-c++ lynx wget bzip2 bzip2-devel bzip2-lib libjpeg-devel make patch openssl-devel zlib-devel epel-release bash-completion


Compilar Python 2.7.10

    $ cd ~
    $ wget https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tgz
    $ tar xvf Python-2.7.10.tgz
    $ cd Python-2.7.10/
    $ ./configure --prefix=/home/USUÁRIO/python-2.7.10/
    $ make
    $ make altinstall

#### Baixe e instale o setuptools atualizado para funcionar através de Proxy Autenticada para esse Python 2.7.10

    $ wget https://bootstrap.pypa.io/ez_setup.py -O - | ~/python-2.7.10/bin/python2.7
    $ unzip setuptools-18.0.1.zip
    $ cd setuptools-18.0.1/
    $ ~/python-2.7.10/bin/python2.7 bootstrap.py


Baixar e rodar o buildout.python

#### Dentro da pasta de projetos

    $ git clone https://github.com/collective/buildout.python.git
    $ cd buildout.python/
    $ ~/python-2.7.10/bin/python2.7 bootstrap.py
    $ ./bin/buildout -vvvt 20

Para o ambiente de desenvolvimento é pressuposto que irão comitar alterações ao repositório e, dessa forma, é necessário ter conta no GitHub e cadastrar a chave pública conforme procedimento [deste link](/ssh/rsa/github/dev/2015/06/11/adicionando-chave-rsa.html "Procedimento para adicionar chave pública no GitHub").

Baixar e rodar o repositório de buildout da Prodam SP Sites

#### Dentro da pasta de projetos

    $ git clone https://github.com/prodamspsites/prodam_buildout.git
    $ cd prodam_buildout/
    $ mkdir downloads/
    $ ../buildout.python/bin/virtualenv-2.7 py27
    $ source ./py27/bin/activate
    $ wget https://bootstrap.pypa.io/ez_setup.py -O - | python
    $ unzip setuptools-18.0.1.zip
    $ cd setuptools-18.0.1/
    $ python bootstrap.py
    $ cd ..
    $ python bootstrap.py -c development.cfg
    $ ./bin/buildout -vvvt 20 -c development.cfg

#### Subir a instância

    $ ./bin/instance fg
