
# Laravel Iniciante - Hour Manager - Parte 1 



Olá galera, tudo tranquilo?!?!?  

Este é o início de uma série de posts para a construção de um aplicativo de gerencimento de horas utilizando Laravel 5.  
Na construção desse aplicativo utilizaremos o básico do Laravel e será um tutorial para iniciantes. Vamos entender o conceito de rotas, controllers, sobre a camada de bando de dados do Laravel, o tão falado **eloquent**.

## Sobre o aplicativo

Qual a intenção de construir um aplicativo em Laravel sem estarmos em uma empresa que nos pague por isso? **APREDIZADO** é a primeira resposta. Mas podemos juntar o necessário ao útil e também ao agradável não é? Então por que não construir algo que possa nos ajudar no dia-a-dia?  

**Como assim?  **

Calma, eu explico. Uma das coisas que sempre tive desde que comecei a trabalhar foi uma planilha no excel que calculava  o número de horas trabalhadas durante um período de tempo. Sempre a boa e velha planilha no excel, que recentemente teve um upgrade para o **Google Docs**, haha. Bom, já que estamos em programadores por que não melhorar essa planilha? Vamos criar um aplicativo onde possamos registrar nossas horas de entrada e saída dos nossos trabalhos (caso você não trabalhe pode registrar suas horas de estudo, o que também é interessante :D)

Então o objetivo do HourManager (estou sem muita criatividade para nomes) é podermos registrar nossos horários de entrada e saída de uma maneira ágil e visualizarmos o número de horas trabalhadas em um certo período. Futuramente iremos tunar o nosso aplicativo, mas por enquanto isso está suficiente.


## Configurando o ambiente

Como ambiente de desenvolvimento vamos utilizar o [Homestead](http://laravel.com/docs/5.0/homestead).  
O **Homestead** é um ambiente de desenvolvimento pronto para que possamos facilmente desenvolver aplicações em Laravel sem precisarmos nos preocupar com configuração de servidores e instalação de recursos.  
O Homestead nada mais é do que uma máquina virtual com software já instalado. Ele utiliza o **Vagrant** para isso, facilitando em muito o nosso trabalho como desenvolvedores.

*Momento nostalgia: Quando digo que facilita muito estou falando sério, me lembro quando comecei com PHP o **inferno** que era ter que instalar o PHP e o Apache no **Windows** (eu sei, é vergonhoso). Hoje em alguns minutos (depende da minha conexão com a internet) já tenho o ambiente de desenvolvimento pronto.*

Falando em Windows, utilizarei um ambiente linux para a instalação e não pretendo nunca mais voltar a programar em Windows (antes que alguém fale, não pretendo programar para desktop ok?). Minha atual distro é o Mint 17, e realmente estou gostando muito, mas isso será em outro post. No caso então, os passos aqui ensinados serão para distros baseadas em ubuntu/debian.

#### PHP

Vamos a configuração do Homestead. Primeiramente vamos instalar o PHP 5.6 em nossas máquinas reais. (Apesar de não ser necessário, é bom, pois poderemos no futuro rodar os testes e outras ferramentas PHP muito mais rápidamente do que dentro da VM)·  
Para instalar o PHP 5.6 é ncessario adicionar um repositório às nossas máquinas:

```
sudo add-apt-repository ppa:ondrej/php5-5.6
```

Após adicionado precisamos atualizar nossos pacotes e instalar o pacote python-software-properties.

```
sudo apt-get update
sudo apt-get install python-software-properties
```

Após isso devemos instalar o PHP 5.6.

```
sudo apt-get install php5-fpm
```

Se não houve nenhum erro durante a instalação devermos ter o nosso PHP 5.6 instalado. Podemos conferir com o comando:

```
php -v
```


#### Composer

Agora o próximo passo é instalarmos o [Composer](https://getcomposer.org/), que é o tão amado gerenciador de dependências para o PHP. A instalação do composer é muito simples, e consiste em baixá-lo e copiar para a pasta **bin** do sistema para que possamos utilizá-lo de qualquer lugar. Os comandos são:

```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

Após a instalação podemos verificar se o composer esta instalado com o comando:
```
composer -V
```

Talvez seja necessário a instalação do pacote curl no sistema:
```
sudo apt-get install curl
```

Por último precisamos adicionar a pasta **bin** do composer ao nosso PATH do sistema para que possamos acessar os aplicativos baixados de qualquer lugar.
```
export PATH=$PATH:~/.composer/vendor/bin/
```

Para que essa alteração entre em vigor basta fecharmos o nosso terminal e abrirmos novamente.


#### Vagrant e Virtualbox

Para a instalação do homestead é necessário a instalação do Vagrant e do Virtualbox. Contudo para a instalação dessa dupla será melhor baixarmos direto do site, pois os pacotes no repositório encontram-se desatulizados.

https://www.vagrantup.com/  
https://www.virtualbox.org/wiki/Downloads

Não há segredo, basta baixar a opção certa para seu sistema (32 ou 64 Bits) e instalá-los. Caso esteja utilizando Mint basta um duplo clique no pacote **.deb**.

#### Homestead

Satisfeitas (ou satisfazidas, como gosto de falar) nossas dependências vamos à instalação de configuração de nosso homestead.
Primeiramente vamos adicionar a box do homestead ao nosso vagrant:

```
vagrant box add laravel/homestead
```

Após isso vamos requisitar o projeto do homestead através do composer:

```
composer global require "laravel/homestead=~2.0"
```

Se o nosso PATH estiver configurado corretamente conforme o passo da instação do **composer** poderemos inicializar nosso projeto Homestead.

```
homestead init
```

Isso criará nosso arquivo de configuração no caminho ```~/.homestead/Homestead.yaml ```. Se esse arquivo foi criado, nosso Homestead está pronto para funcionar :D.


## Finalizando

Bom, hoje foi um artigo abordando o projeto que estamos começando e a instalação de nosso ambiente de desevolvimento. No próximo artigo iremos configurar nosso Homestead e também criar o esqueleto de nossa aplicação, com direito a **registro** e **login**. Até a próxima parte e qualquer dúvida/sugestões/críticas deixem nos comentários ;).

[Laravel Iniciante - Hour Manager - Parte 2](Link para a parte 2 aki)

