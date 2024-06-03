# Avaliação de desempenho e potencial - ROBOFLEX
## Instalando o composer.
### Se você ainda não instalou as dependências do Composer para o seu projeto Laravel, siga esses passos.
**Crie uma pasta no seu computador, abra ela no terminal, e execute esse comando:**
 ```sh
   composer install
   ```

## Instalando laravel
 **Execute o seguinte comando para criar o projeto:**
```sh
   composer create-project laravel/laravel avaliacao-de-desempenho
   ```
### Agora vamos começar a trabalhar dentro do laravel, eu escolhi o VS Code para codificar.
 **AInda no terminal dentro do caminho da pasta, para rodar seu programa em algum navegador:**
  ```sh
   php artisan serve
   ```

## Criando uma classe
### O nosso projeto contém várias classes que no laravel, terão uma tabela, um controller e um model. Vamos começar trabalhando com a classe de Dimensões.
**Criando o controller DimensãoController:**
```sh
   php artisan make:controller DimensaoController --resource
   ```
**Criando o model e a tabela:**
```sh
   php artisan make:model Dimensao -m
   ```
**Adicionando no banco de dados:**
```sh
   php artisan migrate
   ```
### Agora vamos trabalhar na tabela do nosso banco de dados. 
**Siga essa rota (database\migrations\2024_05_20_115249_create_dimensaos_table.php). Após isso, adicionando os atributos necessários, teremos anossa tabela assim:**
```php
        Schema::create('dimensao', function (Blueprint $table) {
            $table->increments('id');
            $table->string('nome');
            $table->timestamps();
   ```

## Controller e model
**Dentro do model faça uma protected $fillable e adicione os atriubutos da sua classe:**
```php
    protected $fillable = [
        'nome'
    ];
   ```

### Na pasta DimensaoController agora vamos fazer as funções de um CRUD(create, read, update, delete).
**No controller recém-criado, você precisa adicionar o método index. Ele irá retornar todos os objetos da classe.**
```php
     public function index()
    {

        return Dimensao::all();
    }

```

**Outro método que iremos utilizar é o store. Esse método será responsável por lidar com a lógica de armazenamento dos dados enviados via uma requisição POST.**
```php
    public function store(Request $request)
    {
        $dimensao = Dimensao::all();
        $validateData = $request->validate([
            "nome"=> 'required|string'
        ]);
        $dimensao = Dimensao::create([
            'nome'=> $validateData['nome']
        ]);
```

**Dentro do seu controller, adicione um método show que receberá o ID como parâmetro e retornará o recurso correspondente.**
```php
      public function show($id)
    {
        $dimensao = Dimensao::find($id);
        return response() -> json(['message'=> 'Dimensão encontrada com sucesso!', 'dimensao'=> $dimensao], 200);
    }
```

**No Controller, adicione um outro método chamado update, que receberá o ID do produto e os dados a serem atualizados.**
```php
     
    public function update(Request $request, string $id)
    {
        $dimensao = Dimensao::find($id);
        $validateData = $request->validate([
            'name'=> 'sometimes|required|string'
        ]);
        $dimensao ->update($validateData);
        return response() -> json(['message'=> 'Dimensão atualizada com sucesso!', 'dimensao'=> $dimensao], 200);
    }
```

**Por último, adicione um método destroy que receberá o ID do produto e o excluirá do banco de dados.**
```php
    public function destroy(string $id)
    {
        $dimensao = Dimensao::find($id);
        $dimensao->delete();
        return response() -> json(['message'=> 'Dimensão removida com sucesso!','dimensao'=> $dimensao], 200);
    }
```

**No final ficará assim:**
```php
     public function index()
    {
        return Dimensao::all();
    }

    public function store(Request $request)
    {
        $dimensao = Dimensao::all();
        $validateData = $request->validate([
            "nome"=> 'required|string'
        ]);
        $dimensao = Dimensao::create([
            'nome'=> $validateData['nome']
        ]);
        
        return response() -> json(['message'=>'Dimensão criada com sucesso!', 'nome' => $dimensao], 200);
        
    }

    public function show($id)
    {
        $dimensao = Dimensao::find($id);
        return response() -> json(['message'=> 'Dimensão encontrada com sucesso!', 'dimensao'=> $dimensao], 200);
    }

    public function update(Request $request, string $id)
    {
        $dimensao = Dimensao::find($id);
        $validateData = $request->validate([
            'name'=> 'sometimes|required|string'
        ]);
        $dimensao ->update($validateData);
        return response() -> json(['message'=> 'Dimensão atualizada com sucesso!', 'dimensao'=> $dimensao], 200);
    }

    public function destroy(string $id)
    {
        $dimensao = Dimensao::find($id);
        $dimensao->delete();
        return response() -> json(['message'=> 'Dimensão removida com sucesso!','dimensao'=> $dimensao], 200);
    }
   ```
## Rotas
### Para cada função, temos que fazer uma rota, que utilizando os verbos necessários, vão nos destinar corretamente.
**Configurando as rotas na pasta routes\api.php:**
```php
   Route::get('dimensao', [DimensaoController::class, 'index']);
   Route::post('dimensao/store', [DimensaoController::class, 'store']);
   Route::get('dimensao/{id}', [DimensaoController::class, 'show']);
   Route::put('dimensao/{id}', [DimensaoController::class, 'update']);
   Route::patch('dimensao/{id}', [DimensaoController::class, 'update']);
   Route::delete('dimensao/{id}', [DimensaoController::class, 'destroy']);
```
### Use o postman para testar o que cada rota está retornando, utilizando de forma correta os verbos GET, POST, DELETE, PUT, PATCH. Então, realize o mesmo processo para todas as classes.

## Configurando chaves estrangeiras
### Na tabela de Competência, temos o atributo 'dimensao_id', que indica a qual dimensão essa competência pertence.
**Para conectar esse atributo com a tabela de dimensões, vamos declarar o atributo assim:**
```php
  $table->integer('dimensao_id')->unsigned();
  $table->foreign('dimensao_id')->references('id')->on('dimensaos')->onDelete('cascade');
```

### No Model vamos declarar os relacionamentos.
**Dentro de Competencia.php**
```php
  public function dimensao()
        {
            return $this->belongsTo(Dimensao::class, 'dimensao_id');
        }
    
```

**No model de Dimensão:**
```php
   public function competencias()
    {
        return $this->hasMany(Competencia::class, 'dimensao_id');
    }
```





