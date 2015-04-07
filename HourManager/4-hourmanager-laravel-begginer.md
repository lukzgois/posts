
# Laravel Iniciante - Hour Manager - Parte 4

Olá pessoal, tudo tranquilo?  

Bem vindos a parte 4 da série sobre a criação de um aplicativo para controle de horas com Laravel 5. Nessa parte criaremos o nosso registro de horas diárias. Empolgados? Vamos nessa então.

#### Objetivo desta parte

O objetivo desse aplicativo é possuirmos uma maneira rápida para registrar nossos horários. Para isso desenvolveremos um botão com o texto "Registrar horário" que será exibido na página inicial do sistema. Ao clicar nesse botão será registrado na tabela o horário atual. O registro do horário entretanto ficará para a próxima parte, nessa focaremos na criação do botão e em alguns conceitos do Laravel (routes, views e controllers).

#### Banco de dados

Para começarmos precisaremos de uma tabela para registrar os nossos horários. Vamos criá-la utilizando as migrations do Laravel. Dentro de nossa VM, na pasta do HourManager vamos executar o comando:

	php artisan make:migration create_hour_registers_table
    
Esse comando irá criar um arquivo novo na pasta **database/migrations**. Esse arquivo iniciará com um registro de data e hora, seguido do nome que indicamos no comando.

Vamos abrir esse arquivo e adicionar o seguinte conteúdo:

```php
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateHourRegistersTable extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('hour_registers', function (Blueprint $table) {
            $table->increments('id');
            $table->date('date');
            $table->time('time');
            $table->timestamps();
        });
	}

	/**
	 * Reverse the migrations.
	 *
	 * @return void
	 */
	public function down()
	{
		Schema::drop('hour_registers');
	}

}

```

Com isso estamos dizendo ao Laravel para criar uma tabela chamada **hour_registers**, com os campos **id (auto_increment)**, **date (date)** e **time (time)**. A função **timestamps()** criará dois campos na tabela: **created_at** e **updated_at**. Esses campos servem para regsitros de edições e são controlados automaticamente pelo framework, por isso não precisamos nos preocupar com eles agora.

Na função **down** estamos apenas destruindo nossa tabela (operação inversa da criação).

O laravel possui vários métodos para criação de campos de diferentes tipos, para saber mais sobre a criação de migrações você pode consultar também a documentação do Laravel em: [http://laravel.com/docs/5.0/schema](http://laravel.com/docs/5.0/schema).

Para finalizar a criação de nosso banco vamos executar o comando:

	php artisan migrate
    
Com isso a tabela **hour_registers** será criada em nosso banco de dados SQLITE.

#### Routes, Controller e Views

O Laravel utiliza o conceito de rotas para definir as URL's de nossa aplicação. Futuramente teremos um artigo dedicado apenas às rotas do Laravel para que possamos entender melhor. Por hora utilizaremos apenas o básico.

Para iniciar vamos excluir a página inicial da nossa aplicação e iniciar diretamente na página de login. Para isso devemos abrir o arquivo **app/Http/routes.php**. Nele temos o seguinte conteúdo:

```php
<?php

/*
|--------------------------------------------------------------------------
| Application Routes
|--------------------------------------------------------------------------
|
| Here is where you can register all of the routes for an application.
| It's a breeze. Simply tell Laravel the URIs it should respond to
| and give it the controller to call when that URI is requested.
|
*/

Route::get('/', 'WelcomeController@index');

Route::get('home', 'HomeController@index');

Route::controllers([
	'auth' => 'Auth\AuthController',
	'password' => 'Auth\PasswordController',
]);

```
Este arquivo é o responsável pelo direcionamento das requisições para suas devidas ações.

Este arquivo foi gerado juntamente com a nossa instalação do Laravel. Podemos ver que uma requisição do tipo **GET** para a url **/** irá nos direcionar para o **WelcomeController** na função **index**. Isso é o que nos faz ver aquela tela inicial do Laravel 5.

Antes de excluirmos essa rota vamos dar uma olhada no **controller** para o qual ela está apontando. Vamos abrir o arquivo **app/Http/Controllers/WelcomeController**. Nele teremos o seguinte conteúdo:

```php
<?php namespace App\Http\Controllers;

class WelcomeController extends Controller {

    /*
    |---------------------------------------------------------------------
    | Welcome Controller
    |---------------------------------------------------------------------
    |
    | This controller renders the "marketing page" for the application and
    | is configured to only allow guests. Like most of the other sample
    | controllers, you are free to modify or remove it as you desire.
    |
    */

	/**
	 * Create a new controller instance.
	 *
	 * @return void
	 */
	public function __construct()
	{
		$this->middleware('guest');
	}

	/**
	 * Show the application welcome screen to the user.
	 *
	 * @return Response
	 */
	public function index()
	{
		return view('welcome');
	}

}
```
Abordaremos sobre o construtor da classe e sobre middlewares mais para frente. Por hora devemos nos ater à função **index**, que é para onde nossa rota esta nos levando. Podemos verificar que essa função está renderizendo uma **view** chamada **welcome**.

Vamos olhar esta **view**, para isso vamos abrir o arquivo **resources/views/welcome.blade.php**. Nele teremos o HTML que é exibido na página inicial.

Esses são 3 conceitos do Laravel: rotas, controllers e view. Nas rotas temos nosso ponto de entrada, lá é decidido para onde uma requisição será direcionada. Após isso fomos mandados para um **controller**. Como o próprio nome diz, eles controlam o que acontecerá ali, quais funções executar e o que responder.  Nosso **controller** respondeu com uma **view**, que é a nossa camada de apresentação. Não misturamos a lógica de nossa aplicação com a camada de apresentação. Nessa parte apresentamos a resposta em HTML, mas poderíamos apresentar em outro formato, por exemplo JSON no caso de uma API.

#### Melhorando a página inicial

Ao decorrer deste tutorial criaremos nossos próprios controllers e views. Por hora vamos excluir o arquivo **app/Http/WelcomeController.php** e o arquivo **resources/view/welcome.blade.php**. Feito isso vamos modificar a linha

```php
Route::get('/', 'WelcomeController@index');
```

para 

```php
Route::get('/', 'HomeController@index');
```

Com isso agora estaremos direcionando nossa rota principal para o controller **HomeController** na função **index**.

Feito isso vamos acessar a nossa rota para testarmos:

	hourmanager.app/
    
Devemos entãoo ser direcionados para a página de login ou para a página incial caso já tenhamos feito nosso login anteriormente.

Passo 1 - Concluído: já temos uma página inicial melhorada. Porém ainda não é o que queremos nesse tutorial. Vamos adicionar nosso botão de registro de horas. Para isso iremos alterar nossa view. 

Abra o arquivo **resources/views/home.blade.php**. Nele temos o seguinte conteúdo:

```html
@extends('app')

@section('content')
<div class="container">
	<div class="row">
		<div class="col-md-10 col-md-offset-1">
			<div class="panel panel-default">
				<div class="panel-heading">Home</div>

				<div class="panel-body">
					You are logged in!
				</div>
			</div>
		</div>
	</div>
</div>
@endsection
```

Vamos remover todo o conteúdo dentro da ```<div class="row">``` e adicionarmos nosso botão. Para isso usaremos os estilos padrão do [Bootstrap](http://getbootstrap.com/css/).

```html
@extends('app')

@section('content')
<div class="container">
	<div class="row">
        <div class="col-md-2 col-md-offset-5">
		    <a href="#" class="btn btn-primary btn-lg">Registrar Agora!!</a>
        </div>
	</div>
</div>
@endsection
```

Feito isso vamos visitar novamente nossa página inicial (caso você não esteja ainda será necessário realizar o login).  
Veremos que agora um botão foi adicionado à esta página (na verdade é um link estilizado como um botão). Contudo, se clicarmos nele nada acontece. Isso porque não informamos um destino para nosso link.  

Vamos utilizar uma função do laravel para criar uma url para o nosso link. Para isso no lugar de 
	
    <a href="#">
    
Troque por

	<a href="{{ url('/register') }}">
    
Se recarregarmos a página e clicarmos no link veremos que fomos redirecionados para a url **hourmanager.app/register** e tomamos uma **exception** bem na nossa cara. (Nossa primeira execption, que emoção).  

Como nosso link já esta funcional vamos analisar o que fizemos:

```php
{{ url('/register') }}
```

Os delimitadores **{{** e **}}** são utilizados pelo **blade**. O **Blade** ~~não é um caçador de vampiros~~ é o **template engine** utilizado pelo Laravel. Em resumo quando criamos uma view com o sufixo **.blade.php** o Laravel entende que ele é um template blade e realiza o processamento antes de exibir na tela.

Quando utilizamos algo entre as chaves duplas({{ e }}) indicamos ao blade que tudo que está ali dentro deve ser tratado como função PHP, o que nos dá acesso a todas as funções do framework também.

A função **url()** cria uma url para um determinado endereço. No nosso caso uma url para o endereço **/register**. 


Vamos falar agora sobre a exceção que acabamos de receber no meio de nossas caras. Ela é uma **NotFoundHttpException**. Essa exceção é lançada pelo framework quando não encontra uma rota compatível com a requisição, o que está mais do que certo, pois não temos uma rota para **/register**. Bora cria-lá então.

#### Nossa primeira Rota

Vamos editar o  arquivo **app/Http/routes.php**. Nesse arquivo adicione a seguinte linha:

```php
Route::get('/register', 'RegisterController@register');
```

Agora temos uma rota para a URL **/register**. Porém se tentarmos acessar essa página receberemos outra exception: **Class App\Http\Controllers\RegisterController does not exist**.  
Essa exception está nos indicando que o controller que estamos tentando redirecionar não existe (o que também é verdade). Nesse caso **#partiuCriarController**.

Dentro de nossa VM na pasta do projetos vamos executar o comando:

	php artisan make:controller RegisterController --plain
    
Após isso vocês verão que temos o arquivo **app/Http/Controllers/RegisterController.php** criado porém sem nenhum método. Se atualizarmos nossa página receberemos outra (mais uma?!?!) exception: **Method [register] does not exist.**  
Essa exception nos informa que o método **register** não existe (tá ai mais uma verdade). Vamos criá-lo dentro do nosso **RegisterController**:

```php
<?php namespace App\Http\Controllers;

use App\Http\Requests;
use App\Http\Controllers\Controller;

use Illuminate\Http\Request;

class RegisterController extends Controller {

	public function register()
    {
        return 'Agora está tudo OK! ;)';
    }

}
```

Agora se tentarmos acessar nossa página veremos nossa frase escrita na tela, indicando que agora nosso roteamento está ocorrendo corretamente. (#aplausos).

## Finalizando

Bom hoje é isso pessoal. Estudamos um pouco sobre rotas, controllers e view, além de que melhoramos nossa página inicial, criamos nossa primeira rota, nosso primeiro controller e nossa primeira migration. Já estamos fazendo progresso. Na próxima parte da série iremos utilizar o **ORM** do Laravel para salvarmos nosso registro de hora no banco e também iremos estudar um pouco sobre **Flash Messages** em nossas views. 

Até lá bons estudos pessoal. #abrass
