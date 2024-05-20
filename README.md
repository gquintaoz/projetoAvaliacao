# Avaliação de desempenho - ROBOFLEX

## Instalando o composer.
**Crie uma pasta no seu computador, abra ela no terminal, e execute esse comando:**
 ```sh
   composer install
   ```

## Instalando laravel
 **Execute o seguinte comando:**
```sh
   composer create-project laravel/laravel avaliacao-de-desempenho
   ```
### Agora vamos começar a trabalhar dentro do laravel, eu escolhi o VS Code para codificar.
 **Para rodar seu programa em algum navegador:**
```sh
   php artisan serve
   ```

## Criando uma classe
### O nosso projeto contém várias classes que no laravel, terão uma tabela, um controller e um model.
**Criando o controller DimensãoController:**
```sh
   php artisan make:controller DimensaoController --resource 
   ```
**Criando o model e a tabela:**
```sh
   php artisan make:model Dimensao -m
   ```
### Agora vamos trabalhar na tabela do nosso banco de dados(database\migrations\2024_05_20_115249_create_dimensaos_table.php). 
**Adicionando os atributos necessários, teremos assim nossa tabela:**
```php
   public function up(): void
    {
        Schema::create('dimensao', function (Blueprint $table) {
            $table->increments('id');
            $table->string('nome');
            $table->timestamps();
        });
    }
   ```
### Controller e model
**Dentro do model faça uma protected $fillable:**
```php
    protected $fillable = [
        'nome'
    ];
   ```
#### No controller agora vamos fazer as funções de um CRUD(create, read, update, delete);
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
        
        return response() -> json(['message'=>'Usúario criado com sucesso!', 'nome' => $dimensao], 200);
        
    }

    public function show($id)
    {
        $dimensao = Dimensao::find($id);
        return response() -> json(['message'=> 'Usuário encontrado com sucesso!', 'dimensao'=> $dimensao], 200);
    }

    public function update(Request $request, string $id)
    {
        $dimensao = Dimensao::find($id);
        $validateData = $request->validate([
            'name'=> 'sometimes|required|string'
        ]);
        $dimensao ->update($validateData);
        return response() -> json(['message'=> 'Usuário atualizado com sucesso!', 'dimensao'=> $dimensao], 200);
    }

    public function destroy(string $id)
    {
        $dimensao = Dimensao::find($id);
        $dimensao->delete();
        return response() -> json(['message'=> 'Usuário removido com sucesso!','dimensao'=> $dimensao], 200);
    }
   ```






