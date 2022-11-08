# Aprendendo Spring Framework

Repositório voltado ao aprendizado do Spring framework. Dessa forma, pode ser utilizado tanto para aprendizado como para consulta.

Atualmente possuo algumas certificações sobre a tecnologia, caso queira consultar [clique aqui](#certificacoes).

#### Como principais fontes bibliográficas foi utilizado:

* [Documentação oficial (EN-US)](https://docs.spring.io/spring-framework/docs/current/reference/html/)
* [Livro: Vire o jogo com Spring Framework](https://www.casadocodigo.com.br/products/livro-spring-framework?_pos=2&_sid=4e54c3220&_ss=r)

## Conceitos
| <!-- --> | <!-- --> | <!-- --> | <!-- --> | 
| :------:|:-----:| :------:| :-------:|
| [1. O que é o Spring framework?](#springframework) | [2. Conceitos prévios?](#conceitos) 	| [3. Injeção de Dependência (ID)](#injecao) |  [4. Inversão de Controle (IOC)](#controle) | 
| [5. Spring Framework Runtime](#springruntime) | [6. Injeção de Dependência no Spring](#injecaospring) | []() | []() | 

<div id='springframework'/>

## 1. O que é o Spring framework
<details>

É um framework Java com o objetivo de facilitar o desenvolvimento de aplicações. Para isso, o framework explora os conceitos de inversão de controle (IoC) e injeção de dependências (ID). Portanto, ao adotá-lo, temos uma tecnologia que não somente fornece os recursos necessários à grande parte das aplicações, como módulos para persistência de dadoos, integração, segurança, testes etc. como também um conceito que permite criar soluções com menos acoplamento e mais coesão.

<summary> Conteúdo: </summary>


</details>

<div id='conceitos'/>

## 2. Conceitos prévios
<details>

<summary> Conteúdo: </summary>

#### Acoplamento
       
Acoplamento é um termo que se refere a conexão ou dependência entre diversos módulos/sistemas de um software. 
       
Os benefícios que o baixo acoplamento proporciona:
* Reusabilidade de componentes, pois um componente interdependente pode ser facilmente implementado em outro sistema;
* Melhor manutenção do sistema, pois a manutenção em um módulo não afeta o restante do sistema;
* Códigos altamente "testáveis", pois é possível testar mais facilmente o código através de "mock tests";
* Códigos mais legíveis, tornando mais fácil a compreensão do sistema como um todo.

#### Coesão

Coesão é um termo que se refere a responsabilidade de um módulo de um software. Dessa maneira, temos como um dos princípios de SOLID a responsabilidade única, que determina que uma classe deve ser responsável por uma única responsabilidade. Pode-se traduzir esse princípio como: "uma classe deve ter apenas um único motivo para mudar"

Os benefícios que a alta coesão proporciona:
* Classes mais fáceis de fazer manutenção;
* Classes mudam com menos frequência;
* Classes mais reutilizáveis.

</details>

<div id='injecao'/>

## 3. Injeção de Dependência (ID)
<details>

<summary> Conteúdo: </summary>

Primeiramente, dependência em programação tem relação com utilizar funcionalidades de outra classe. Ou seja, quando a classe Foo usa alguma funcionalidade da classe Bar, diz-se que a classe Foo tem uma dependência da classe Bar. 
       
Em java, é necessário criar o objeto daquela classe antes de usar o método de outras classes, assim, é necessário que a classe Foo crie uma instância da classe Bar. Dessa maneira, transferir a tarefa de criação do objeto para outra entidade e usar diretamente a dependência é chamado de injeção de dependência.
       
A responsabilidade da injeção de dependência é:
1. instanciar os objetos;
2. Identificar as classes que necessitam desses objetos;
3. Fornecer todos esses objetos.
       
#### Benefícios da ID : 

A principal vantagem é a possibilidade de mudar features do objeto em tempo de execução em vez de tempo de compilação, pois as dependências podem ser injetadas em tempo de execução.    
       
### Tipos de injeção de dependência
       
Pode-se afirmar que existem 3 tipos de ID:
* Injeção de construtor;
* Injeção pelo setter;
* Injeção de interface;

#### Injeção de construtor :
>*Injeção de Construtor é o ato de definir estaticamente a lista de Dependências necessárias , especificando-as como parâmetros para o construtor da classe.*

Essa forma de aplicar ID é a mais comum. Nela cria-se um construtor que tem como papel, obrigar o sistema a especificar os parâmetros necessários.

O código abaixo visa demonstrar como é aplicado :
```java
public class FooController{
       private readonly BarService service;                           //1
                                                                      
       public FooController (BarService service){                     //2
              if (service==null)                                      //3
                     throw new ArgumentNullException("service");      //3
              this.service = service;                                 //4
       }
}
```

Papel das respectivas linhas:
* 1: Campo da instância privada com o objetivo de armazenar a dependência fornecida;
* 2: Construtor que define estaticamente suas dependências e o argumento para fornecer a dependência;
* 3: Cláusula para evitar a passagem de dependências em null;
* 4: Armazenar a dependência no campo privado para uso posterior.
       
#### Injeção pelo setter :
       
Essa forma de aplicar ID
       
O código abaixo visa demonstrar como é aplicado:
```java
public class Foo {
       private Bar bar;            //1
       
       public Foo() { }            
       
       public Bar getBar() {       //2
              return this.bar;
       }
       
       public setBar(Bar bar){     //3
              this.bar = bar;
       }
       
       public FooBar(){            
              bar.metodo();
       }
}
```

Papel das respectivas linhas:
* 1: Campo da instäncia privada com o objetivo de armazenar a dependência fornecida;
* 2: Método que busca a dependência já implementada anteriormente;
* 3: Método que implementa a dependência na variável;

#### Injeção de interface :

O Spring framework não suporta injeção por interface atualmente, portanto, no momento não me aprofundarei nos conceitos de injeção por interface.
       
       
       
Caso queira se aprofundar, recomendo o seguinte [artigo.](https://www.martinfowler.com/articles/injection.html)
       
</details>

<div id='controle'/>

## 4. Inversão de Controle (IOC)
<details>

<summary> Conteúdo: </summary>

A Inversão de controle (IOC) é um modo de manipular o controle sobre um objeto. A seguir, o exemplo de um código sem inversão de controle: 
```java
public class Foo { 
       public void metodo1(Obj obj) { 
              Bar bar = new Bar("Texto.txt"); 
              bar.metodo2(obj); 
       }
} 
```

Problema: 
A classe Foo depende do nome do arquivo (parâmetro do construtor para instanciar o objeto); caso o nome do arquivo mude, é necessário abrir a classe Foo para alterar algo da classe Bar. Resumindo: para alterar algo da classe Bar, tivemos que acessar a classe Foo. Pode parecer trivial, mas caso tivessemos centenas de classes que dependem do Bar, poderíamos acabar esquecendo de trocar alguma. 

Resolvendo: 
Em vez da classe foo ser responsável pela criação do Bar, fazendo a inversão de controle (ou digamos: inversão de responsabilidade), e para isso, usaremos uma injeção de dependência. 
Portanto, injeção de dependência é uma forma para resolver a inversão de controle. 
No código abaixo iremos usar a técnica Constructor Injection, ou seja, injetamos uma dependência de classe através de um construtor desta classe. 

 
```java
public class Foo{ 
       private Bar bar; 

       public Foo(Bar bar1){ 
              This.bar = bar1; 
       } 

       public void metodo1(Obj obj) { 
              bar.metodo2(obj); 
       } 
} 
```
 

Ou seja, em vez da classe foo instanciar o objeto da classe Bar dentro de seu método, ela já recebeu esse objeto instanciado!  
Dessa forma, a classe Foo só utiliza a classe Bar sem se preocupar com sua criação. A classe Foo não precisa saber como a classe Bar foi instanciada, só precisa receber a instância. 

Assim, diminuí o acoplamento entre as classes e facilita a manutenção do código, além da elegância e possibilidade de gerar testes das classes! 

</details>


<div id='springruntime'/>

## 5. Spring Framework Runtime
<details>

<summary> Conteúdo: </summary>

<div>
  <img align="center" alt="Pixel-Art" width="100%" src="https://github.com/AlexAlbert-Dev/Learning-Spring/blob/72b4316561fa2322db00c0a09bf0c1111855c533/SpringRuntime.png"/>
</div>

</details>

<div id='injecaospring'/>

## 6. Injeção de Dependência no Spring
<details>

<summary> Conteúdo: </summary>

### Introdução

A injeção de dependência no Spring é através de um container chamado Spring IoC Container. Tal container é o responsável por gerenciar todas as dependências do projeto de forma automática, e esses objetos gerenciados pelo container do Spring são chamados de Beans. Não há distinção entre os beans e os objetos utilizados normalmente no projeto, com exceção de que eles são gerenciados pelo IoC Container.
       

Existem diversas formas de declarar um bean do Spring, sendo a mais conhecida a anotação @Component. Outras anotações também podem ser utilizadas, como por exemplo: @Controller, @Service, @Repository, etc.

A seguir o exemplo de uma classe declarada como um componente Spring (bean):

```java
@Component
public class FooService {
       private Bar bar;
       
       public FooService (Bar bar){
              this.bar = bar;
       }
       
       public FOOBAR metodo(Produto produto){
              return this.bar.metodo2(produto);
       }
}
```

### Configuração de Beans

O Spring por padrão instância os beans sem nenhum tipo de configuração. Mas caso seja necessário, é possível fazer sua configuração criando uma classe Java com a anotação @Configuration e definir métodos com os nomes dos beans a serem instanciados e gerenciados pelo IoC Container.

A seguir um código exemplo dessa configuração:

```java
@Configuration
public class FooConfigo{
       @Bean
       public Bar bar() {
              FooBar foobar = new FooBar();
              return new Bar(foobar);
       }
}
```
Dessa forma, toda vez que o Spring precisar instanciar a classe Bar, ele ir[a chamar o m[etodo anotado com @Bean, que constrói o objeto de forma personalizada.

### Pontos de Injeção

Caso a classe tenha uma sobrecarga, é necessário indicar para o Spring qual é o ponto de injeção que ele deve utilizar, e isso é realizado através da anotação @Autowired.

A seguir um código exemplo:

```java
@Component
public class Foo {
       private Bar bar;
       
       @Autowired
       public Foo (Bar bar) {
              this.bar = bar;
       }
       
       public Foo (String tks) {
              //Sobrecarga de construtor
       }
       
       public FooBar foobar (Produto produto) {
              return this.bar.foobar2(produto);
       }
}
```

### Desambiguação de Beans

Ambiguação de Beans pode ocorrer em cenários como o descrito abaixo:

```java
@Component
public class Foo1 implements Bar {
       public Classe metodo (Produto produto){
              //Qualquer coisa
       }
}

@Component
public class Foo2 implements Bar {
       public Classe metodo (Produto produto){
              //Qualquer coisa
       }
}
       
@Component
public class FooService{
       @Autowired
       private Bar bar;
       
       public Classe metodo2 (Produto produto) {
              // Qualquer coisa
              return this.bar.metodo(produto);
       }    
```

O Spring náo saberá qual das implementações de Bar deverá injetar no objeto da classe FooService, causando assim uma exceção de ambiguidade.       
       
Uma forma de realizar a desambiguação de Beans é utilizando a anotação @Qualifier. Essa anotação permite definir um nome para identificar o Bean. A seguir o código implementando a anotação na situação descrita acima:
       

```java
@Qualifier("Foo1")
@Component
public class Foo1 implements Bar {
       public Classe metodo (Produto produto){
              //Qualquer coisa
       }
}
       
@Qualifier("Foo2")
@Component
public class Foo2 implements Bar {
       public Classe metodo (Produto produto){
              //Qualquer coisa
       }
}
       
@Component
public class FooService{
       @Qualifier("Foo2")
       @Autowired
       private Bar bar;
       
       public Classe metodo2 (Produto produto) {
              // Qualquer coisa
              return this.bar.metodo(produto);
       }    
```


</details>

<div id='tiposinjecaospring'/>


## Minhas certificações sobre o assunto
[Java Servlet: programação web Java]()
