# Avaliaçõa de desempenho - ROBOFLEX
## Criando o arquivo
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






