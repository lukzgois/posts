# Laravel Iniciante - Hour Manager - Parte 2

E ai pessoal, como vão, empolgados?

Essa é a segunda parte da nossa série para iniciante em Laravel 5. Nesse episódio vamos configurar nosso Homestead e também criar o esqueleto inicial de nossa aplicação.

##Configurando o Homestead

Vamos mapear nossa máquina virtual para que possamos acessar nossos projetos da nossa máquina real. Para isso primeiramente precisaremos configurar nossas chaves SSH.  
Caso você já tenha criado suas chaves SSH pode pular para o próximo passo.


#### Chaves SSH

Para criar nossas chaves SSH usaremos o comando:
```
ssh-keygen -t rsa -C "seu@email.com"
```

Nas perguntas que seguirem podemos apenas apertar enter. Feito isso teremos um par de chaves localizados na pasta ```~/.ssh```.  
O arquivo ```id_rsa``` é nossa chave privada, e deve ser mantida na nossa máquina. O arquivo ```id_rsa.pub``` é nossa chave pública, e servirá para compartilharmos em nossos projetos.

Criadas as chaves vamos então criar as nossas

 
#### Criando as pastas

Para organizar nosso projeto vamos criar uma pasta chamada **Develpoment** onde organizaremos nossos projetos (sim, haverão outros hehe :D).

Então para organização desses projetos vamos criar essa pasta:
```
mkdir ~/Development
mkdir ~/Development/HourManager
```

#### Criando os hosts fake

Para acessarmos nossa aplicação vamos utilizar um host fake que será redirecionado para dentro do nosso Homestead. Para isso basta adicionarmos a seguinte linha ao arquivo ```/etc/hosts```.

```
192.168.10.10	hourmanager.app
```

#### Configurando o Homestead

Vamos editar o arquivo de configuração do homestead. Para isso basta editarmos o arquivo ```~/.homestead/Homestead.yaml```.

```vim ~/.homestead/Homestead.yaml```

```
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/Development/HourManager
      to: /home/vagrant/HourManager

sites:
    - map: hourmanager.app
      to: /home/vagrant/HourManager/public

databases:
    - homestead

variables:
    - key: APP_ENV
      value: local
```

A diretiva folders indica as pastas que serão mapeadas da nossa máquina real para a nossa máquina virtual. No nosso caso estamos mapeando a pasta ```~/Development/HourManager``` para a pasta ```/home/vagrant/HourManager``` dentro da VM.

A diretiva sites indica os hosts do nginx que serão criados na nossa máquina. Preciamos apontar os sites para a pasta pública da nossa aplicação. No caso do laravel, sempre a pasta public na raiz do projeto. Lembrando que o Homestead não serve apenas para aplicações Laravel, podemos colocar qualquer aplicação PHP dentro do Homestead.

####Iniciando a VM

Feitas as configurações necessárias basta iniciarmos nossa VM com o comando:

```
homestead up
```
Se tudo correr bem, teremos nossa máquina virtual rodando. Para acessá-la utilizaremos o comando

```
homestead ssh
```

Para desligá-la basta sairmos dela e utilizarmos o comando:

```
homestead halt
```


## Criando o projeto

Se você for ao seu browser preferido (recomendo Firefox para causar polêmica HAHAHAHA) e acessar a URL **hourmanager.app** você verá apenas uma mensagem de 

```
File not found
```

Isso acontece porque não possuimos nenhum arquivo em nossa pasta. Vamos criar nosso projeto agora (\o/). Como já possuimos o **composer** instalado vamos criar o nosso projeto da nossa máquina real. Vamos gerar a aplicação na pasta que criamos no início do tutorial:

```
cd ~/Development/HourManager
composer create-project laravel/laravel --prefer-dist .
```

Esse comando irá iniciar uma nova aplicação Laravel na pasta atual. Após finalizado teremos uma aplicação novinha em folha.

Vamos novamente acessar nossa url **hourmanager.app** para ver o que acontece.

Nesse ponto devemos ter uma tela de boas vinda do Laravel para nós, indicando que começamos agora a construir código bonito e divertido. Parabéns, garanto a todos que essa é uma viagem sem volta HAHAHA.

## Finalizando

Sei que todos devem estar ansiosos para programar, mas iremos continuar no próximo capítulo, onde iremos configurar nosso banco, criar nossos usuários e também verificar o formulário de login. Até breve (muito breve) pessoal.
