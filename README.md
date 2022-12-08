# Entendendo Spring Framework

Repositório voltado ao entendimento do Spring framework, criado a partir das anotações dos meus estudos do assunto. Dessa forma, pode ser utilizado tanto para aprendizado como para consulta.

Atualmente possuo algumas certificações sobre a tecnologia, caso queira consultar [clique aqui](#certificacoes).

#### Como principais fontes bibliográficas foi utilizado:

* [Documentação oficial (EN-US)](https://docs.spring.io/spring-framework/docs/current/reference/html/)
* [Livro: Vire o jogo com Spring Framework](https://www.casadocodigo.com.br/products/livro-spring-framework?_pos=2&_sid=4e54c3220&_ss=r)

## Conceitos
| <!-- --> | <!-- --> | <!-- --> | <!-- --> | 
| :------:|:-----:| :------:| :-------:|
| [1. O que é o Spring framework?](#springframework) | [2. Conceitos prévios?](#conceitos) 	| [3. Injeção de Dependência (ID)](#injecao) |  [4. Inversão de Controle (IOC)](#controle) | 
| [5. Bean e JavaBean](#javabeans)| [6. Spring Framework Runtime](#springruntime) | [7. Injeção de Dependência no Spring](#injecaospring)  | [8. Injeção de Dependência no Spring - Declarações no XML](#declarandobeanXML) | 
|[9. Utilização do Container](#utilcontainer)|[10. Configurações do XML](#configXML)|[11. Spring Expression Language - SpEL](#springel)|[12. Autowiring](#autowiring)|
|[13. Anotações do Spring](#anotacoes)|[]()|[]()|[]()|

<div id='springframework'/>

## 1. O que é o Spring framework
<details>

<summary> Conteúdo: </summary>

É um framework Java com o objetivo de facilitar o desenvolvimento de aplicações. Para isso, o framework explora os conceitos de inversão de controle (IoC) e injeção de dependências (ID). Portanto, ao adotá-lo, temos uma tecnologia que não somente fornece os recursos necessários à grande parte das aplicações, como módulos para persistência de dadoos, integração, segurança, testes etc. como também um conceito que permite criar soluções com menos acoplamento e mais coesão.

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

#### POJO

Um POJO, em inglês Plain Old Java Object, é uma classe Java comum mas que não necessita implementar nenhuma interface ou possuir determinada anotação para que possa ser gerenciada por algum framework. Assim, podemos afirmar que é a classe que implementa as regras de negócio da forma ideal, ou seja, só se preocupa exclusivamente com os requisitos funcionais.

#### Lookup

Esse termo faz referência a obtenção de objetos a partir de um container. Temos que todo container precisa fornecer alguma interface que forneça métodos para obtenção de objetos a partir de nome ou tipo.

#### Código Boilerplate

Se refere a partes de código que devem ser incluídas em diversos lugares com pouquissíma ou nenhuma alteração.

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

<div id='javabeans'/>

## 5. Bean e JavaBean
<details>

<summary> Conteúdo: </summary>

#### Bean

Tenhamos em mente um objeto de negócio, sendo o mesmo uma instância de uma classe onde é implementado nossos requisitos funcionais. O Enterprise JavaBeans (EJB) é um objeto de negócio, e no contexto do Spring Framework, chamamos de Bean. Portanto, um bean é um objeto que seu ciclo de vida é gerenciado pelo container de IoC/DI do SPring.

#### JavaBean

JavaBean é um padrão que foi adotado para a escrita de componentes reutilizáveis. Esse padrão explícita que todo JavaBean deve respeitar três regras:

1. Deve possuir um construtor público e que não receba parâmetros;
2. Todos os seus atributos visíveis devem ser privados ou protegidos, e acessados somente através de métodos get e set;
3. Devem implementar a interface java.io.Serializable.

Dessa forma, um JavaBean deve aparentar isso:

```java
public class Foo implements java.io.Serializable {
       private String bar;
       
       public Foo() {}
       
       public String getBar() {
              return this.bar;
       }
       
       public void setBar(String bar) {
              this.bar = bar;
       }
}
```

</details>

<div id='springruntime'/>

## 6. Spring Framework Runtime
<details>

<summary> Conteúdo: </summary>

O Spring Framework é dividido da seguinte forma:

<div>
  <img align="center" alt="Pixel-Art" width="100%" src="https://github.com/AlexAlbert-Dev/Learning-Spring/blob/72b4316561fa2322db00c0a09bf0c1111855c533/SpringRuntime.png"/>
</div>

### Core Container

Também conhecido como núcleo do Spring, ele é responsável por criar os objetos, configurá-los e gerenciar seu ciclo de vida por completo. Para isso, o Core Container é separado em quatro  componentes.

#### Core

Onde se localiza toda a infraestrutura necessária para a implementação do container. Aqui pode ser encontrado facilitadores no gerenciamento de classloaders, tratamento de recursos, strings etc.

#### Beans

Aqui é onde se localiza o BeanFactory, responsável pela implementação do factory. O factory em si é um padrão de projetos que permite às classes delegarem para as suas subclasses decidirem quais objetos criar. Portanto, é este padrão que permite o desacoplamento total do gerenciamente de dependência e ciclo de vida dos beans através do mecanismo de configuração que é implementado neste módulo.

#### Context

Onde se encontra a implementação mais avançada e utilizada do container: ApplicationContext. Sendo o mesmo a interface central de um aplicativo Spring, usado para fornecer informações de configuração ao aplicativo. Essa interface oferece recursos poderosos como a internacionalização, AOP, gerenciamenteo de recursos, propagação de eventos e acesso a funcionalidade básicas da plataforma Java EE.

#### SpEL

É uma linguagem baseada em expressões que pode ser utilizado como modo de configuração do Spring. Sendo essa linguagem responsável por permitir definir comportamentos e valores aos nossos beans definidos usando XML.

### AOP e Aspects

Spring também permite programar orientado a aspectos, ou seja, ele tem seu próprio framework integrado que permite implementar a programação orientada a aspectos. Assim sendo, ele tem duas formas de programar orientado as aspectos, sendo:

* Tradicional: primeira implementação de AOP do Spring;
* Aspects: implementação mais moderna, que faz uso do AspectJ como motor de AOP, sendo bem mais poderoso que o tradicional.

### Instrumentation

Tendo em mente que a maior parte do custo de um software é a sua manutenção, este módulo facilita a vida da equipe de suporte, pois oferece facilidades na implementação de JMX. Sendo que esta tecnologia que permite acompanhar em tempo de execução tudo que está ocorrendo com nossos sistemas.

### Messaging

### Data Access/Integration

Esse módulo que permite o Spring oferecer suporte às principais tecnologias de persistência adotadas pelos desenvolvedores Java (JDBC, ORM, OXM, JMS, Transactions). Assim, o suporte a estas tecnologias se dá através de templates, que vão reduzir significativamente a quantidade de código boilerplate, ou seja, de infra estrutura, como por exemplo: abrir e fechar conexões, iniciar e finalizar transações etc.

Além disso, este módulo contém o controle transacional. O controle transacional permite que o Spring oferença uma implementação de transações declarativas e programáticas, tornando obsoleto a implementação de transações oferecidas pelos servidores de aplicação.


#### JDBC
       
JDBC é um conjunto de classes e interfaces escritas em Java que executam o envio de instruções SQL para um banco de dados relacional. Portanto, esse módulo visa facilitar a implementação de repositórios baseados em JDBC, dando suporte aprimorado as camadas de acesso e dados baseadas em JDBC. Cabe ressaltar que como o Spring Data JDBC visa ser simples, ele não oferece: cache; carregamento lento; write-behind etc. Tornando-o um ORM simples e limitado.

#### ORM
       
#### OXM

#### JMS

#### Transactions



### Web

#### WebSocket

#### Servlet

#### Web

#### Portlet

</details>

<div id='injecaospring'/>

## 7. Injeção de Dependência no Spring
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


<div id='declarandobeanXML'/>

## 8 Injeção de Dependência no Spring - Declarações no XML

<details>

<summary> Conteúdo: </summary>

### Introdução

Um bean necessita de três elementos para existir: configurações, container e a classe que implementa o mesmo. Portanto, nesse tópico iremos tratar das configurações e temos a possibilidade de configurar um bean de três formas diferentes: XML, anotações ou código Java.

O modo de configurar mais educativo é o XML, pois explícita muito mais que as anotações ou código Java.


### Declarando um Bean

Para declarar um bean é utilizado a tag ```<bean>```, onde a única informação obrigatória é a classe. Um exemplo de declaração de Bean implementando uma interface:

```xml
<bean id = "nomeInterface" class="br.com.alexalbert-dev.nomeInterfaceArquivos" />
```
 Onde:
 * id = define um identificador único ao bean;
 * class = classe que o implementa, ou seja, sua classe de origem.
 
Aliás, tendo em mente que um bean só está disponível após sua inicialização, o container consegue resolver os problemas decorrentes da obtenção concorrente de objetos, que ocorre quando uma inicialização não foi totalmente concluída.

### Injeção de Dependência por Setters

Um bean é de pouca utilidade, mas a união de vários aplicando a injeção de dependência mostra a verdadeira utilidade do Spring Framework. Um exemplo de injeção por setters abaixo:

```xml
<beans xmlns="http://..."
       xmlns:xsi="http://..."
       xsi:schemaLocation="http://...">
       
       <bean id = "nomeInterface" class="br.com.alexalbert-dev.nomeInterfaceArquivos" />
              <property name="arquivo" value="/arquivos/qualquerArquivo" />
       </bean>
       
       <bean id="nomeImplementacao" class="br.com.alexalbert-dev.nomeImplementacao" />
       
       <bean id="classeMain" class="br.com.alexalbert-dev.classeMain">
              <property name="implementacao" ref="nomeImplementacao" />
              <property name="classeArquivo" ref="nomeInterface" />
       </bean>            
</beans>
```

Onde:
* xmls, xmlns:xsi e xsi:schemaLocation = declaração de namespaces, gerado automaticamente por IDE's que dão suporte ao Spring;
* ``` <property> ``` = tag para modificar atributos no objeto (bean);
* ``` <property name="..." > ``` = define o atributo name do objeto, fazendo referência ao padrão JavaBean;
* ``` <property value="..." > ``` = seguindo o padrão JavaBean, é necessário que a classe tenha um set, e valor a ser injetado nesse set é este atributo. Cabe ressaltar que não é necessário saber o tipo do setter, pois ele será descoberto em tempo de execução;
* ``` <property ref="..." > ``` =  o valor é de algum bean definidos na configuração, sendo essa a injeção de dependência.

### Injeção de Dependência por Construtor

Um exemplo de injeção de dependência por construtor:

```xml
       // Declaração no XML
<bean id="Foo" class="Bar">
       <constructor-arg ref="FooBar" index="1">
       <constructor-arg ref="BarFoo" index="0">
</bean>
```
```java       
       // Construtor na classe
public Foo (BarFoo barfoo, FooBar foobar) {
       setBarFoo(barfoo);
       setFooBar(foobar);
}
```

Onde:
* ``` <constructor-arg> ``` = tag responsável por executar a injeção por construtor;
* ``` <constructor-arg ref="..."> ``` = pode receber como atributo value ou ref;
* ``` <constructor-arg index="..."> ``` = os constructor arg devem seguir a ordem dos parâmetros do construtor, caso queira alterar a ordem, é necessário explicitar a ordem dessa forma.

</details>

<div id='utilcontainer'/>

## 9. Utilização do Container

<details>

<summary> Conteúdo: </summary>

### Utilização do container

O container do Spring possui duas versões: BeanFactory e o ApplicationContext. Sendo que o ApplicationContext é a evolução do BeanFactory, e segundo a propria equipe do Spring, é altamente recomendável utilizar o ApplicationContext, cabendo utilizar a outra versão somente em situações de restrições computacionais extremas (como sistemas embarcados e applets) e mesmo assim, o BeanFactory existe ainda principalmente para fim de retrocompatibilidade.

Um artigo interessante para um aprofundamento em suas diferenças é esse [aqui.](https://www.geeksforgeeks.org/spring-difference-between-beanfactory-and-applicationcontext/)

A estrutura base do ApplicationContext é a seguinte:

<div>
  <img align="center" alt="Pixel-Art" width="100%" src="https://github.com/AlexAlbert-Dev/Learning-Spring/blob/e8649681ba167f6a31212d52d8ef4d33e49fb641/ApplicationContext.png"/>
</div>

</details>


<div id='configXML'/>

## 10. Configurações do XML

<details>

<summary> Conteúdo: </summary>

### Introdução

As configurações do XML nos permite configurar a estado inicial e o comportamento de nosso beans, permitindo que o container consiga administrar plenamente o ciclo de vida dos nossos beans. Cabe ressaltar que hoje em dia essas configurações se tornaram um pouco obsoletas, pois as anotações do Spring se desenvolveram a ponto de substituir o xml. No entanto, para aprendizado e entendimento do funcionamento interno do container é uma excelente ferramenta.

### Instanciação por Factory Method

Inicialmente é necessário entender o conceito de *Factory*, sua definição mais famosa pode ser encontrada no livro "Padrões de Projeto: Soluções reutilizáveis de software orientado a objetos". O conceito se resume à uma classe, que é chamada de factory, e ela é responsável pela instanciação de novos objetos. 

Cabe ressaltar que toda classe *factory* possui pelo menos um método instanciador. Além disso, podemos identificar o conceito de inversão de controle em uma classe factory, pois é delegada a ela a responsabilidade de escolher qual é a implementação correta a ser obtida.

Uma instanciação por factory method pode ser realizada de duas formas: estática ou não. A seguir iremos ver ambas as formas.

#### Instanciação por método estático

Imagine uma classe onde iremos implementar um método estático "criaEstatico", responsável por obter um parâmetro e identificar a partir de qual classe deve ser instanciado o objeto. O código ficaria da seguinte forma:

```xml
<bean id="identificador" class="br.com.exemplo.FactoryExemplo" factory-method="criaEstatico">
       <constructor-arg index="0" value="parametroEntrada"/>
</bean>
```

Dessa maneira, como o método estático recebe parâmetros diretamente, reaproveitamos a tag ```<constructor-arg>```, onde passamos o valor do tipo bean que desejamos como retorno.

#### Instanciação por método não estático

Imagine uma classe onde iremos implementar um método não estático "criar", ele vai ser invocado por algum bean que o implemente. O código do nosso bean fica da seguinte maneira:

```xml
<bean id="factory" class="br.com.exemplo.FactoryExemplo"/>

<bean id="instancia" factory-bean="factory" factory-method="criar">
       <consctructor-arg value="random"/>
</bean>
```

É considerado um método não estático pois o tipo do objeto bean será decidido em tempo de execução. Portanto, é desnecessário incluir o atributo *class* na tag ```<bean>```.


### Mapeamento de atributos complexos

Para definirmos integralmente o estado de início dos beans, é necessário que a tag ```<property>``` seja capaz de definir valores além dos primitivos e strings, possibilitando o mapeamento de listas, mapas, instâncias do java.util.Properties e arrays.

#### Listas

para o entendimento de listas usaremos dois exemplos: um com um ArrayList simples e outro utilizando uma lista de instâncias do tipo java.io.File.

##### Exemplo 1:

De início, vamos criar uma classe que implemente um interface:

```java
class ListasDoProjeto implements Dados {
       private List<String> frases;
       
       public List<String> getLinhas() {return frases;}
       public void setLinhas(List<String> paragrafo) {frases=paragrafo}
}
```

A configuração correta da tag ```<property>``` para mapear ficaria da seguinte forma:

```xml
<bean class="br.com.exemplo.ListasDoProjeto">
       <property name="listas">
              <list>
                     <value>/caminho/contem/paragrafo1.csv</value>
                     ...
                     <value>/caminho/contem/paragrafoN.csv</value>
              </list>
       </property>
</bean>
```

Cabe relembrar que a tag ```<value>``` aceita primitivos e strings.

##### Exemplo 2:

De início, vamos criar uma classe que implemente uma instância lista do tipo java.io.File:

```java
class ListasDoProjeto implements Dados {
       private List<java.io.File> frases;
       //resto da classe acima
}
```

Sabendo que a tag ```<list>``` aceita a tag ```<bean>``` em seu interior, a configuração correta do xml ficaria:

```xml
<bean class="br.com.exemplo.ListasDoProjeto">
       <property name="listas">
              <list>
                     <bean class="java.io.File">
                            <constructor-arg value="/caminho/contem/paragrafo1.csv"/>
                     </bean>
                     
                     ...
                     <bean class="java.io.File">
                            <constructor-arg value="/caminho/contem/paragrafoN.csv"/>
                     </bean>
              </list>
       </property>
</bean>
```

Observação: se o bean não for anônimo, pode-se utilizar a tag ```<ref>``` apontando seu id.

#### Mapas

Veremos um exemplo de mapeamento de dados utilizando uma estrutura do tipo java.util.Map.
       
```java
public class MapasDoProjeto implements Dados {
       private Map<String, Object> candidato;
       
       public Map<String, Object> getCandidato() {return candidato;}
       public void setMapa(Map<String, Object> novoCandidato) {candidato=novoCandidato;}
}
```

A configuração correta do seu xml seria:

```xml
<bean name="mapas" class="br.com.exemplo.MapasDoProjeto">
       <property name="candidato">
              <map>
                     <entry key="arquivoCandidato">
                            <ref bean="arquivoCandidato">
                     </entry>
                     <entry key="arquivoCandidatoPorValor" value="/caminho/arquivoCandidato.csv"/>
              </map>
       </property>
</bean>
```

Dentro da tag ```<map>``` utilizada para configurar um mapa, temos a tag ```<entry>``` que indica a entrada no mapa. Assim, o atributo key faz referencia a identificar sua chave. No interior são aceitos as seguintes tags: ```<bean>```, ```<value>```, ```<list>```, ```<array>``` e ```<map>```.

#### Instâncias do Properties

Uma das classes mais utilizadas é a java.util.Properties. O Spring dá suporte especial à ela e como exemplo prático (encontrado no livro "Vire o jogo com Spring Framework") analisaremos a configuração xml da classe SessionFactory do Hibernate.       

```xml
<bean id="sessionFactory" class="org.springframework.orm.hibernate.LocalSessionFactoryBean">
       <property name="hibernateProperties">
              <props>
                     <prop key="hibernate.dialect">org.hibernate.dialect.MySQL5InnoDBDialect</prop>
                     <prop key="hibernate.hbm2ddl.auto">update</prop>
              </props>
       </property>
</bean>                      
```

A tag ```<props>``` é a representação de um objeto do java.util.Properties, e portanto, a tag ```<prop>``` mapeia as propriedades do objeto. Esse exemplo é melhor explicado no livro utilizado na bibliografia.

### Instanciando um container

A diferença entre as variações do ApplicationContext, classpath e filepath, está na fonte utilizada para obter os documentos XML responsáveis por configurar nossos beans, sendo respectivamente: o classpath da aplicação e o sistema de arquivos local onde o projeto é executado.

O exemplo abaixo instancia um container que busca o arquivo projeto.xml utilizando o classpath:

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

       // oculto informações desnecessárias da classe
       ApplicationContext context = new ClassPathXmlApplicationContext("projeto.xml");
       // oculto informações desnecessárias da classe
```


### Obtendo os beans

Considerando a configuração anterior, para obter um bean *candidato* basta usar a função *getBean*, da seguinte maneira:
       
```java
ApplicationContext context = new ClassPathXmlApplicationContext("projeto.xml");
       Candidato bean = (Candidato) context.getBean("candidato");
       bean.cadastraCandidato();
```

Mas como quase tudo no Spring, temos uma flexibilidade de modos de fazer as coisas, e portanto, existem duas versões do método *getBean*, e iremos tratar de ambas as formas.
       
1. Utilizando type casting manual
       
```java
Candidato bean = (candidato) context.getBean("candidato");
```
       
2. Utilizando generics
       
```java
Candidato bean = context.getBean("candidato", Candidato.class);
```
       
#### Buscando bean anônimo
       
Caso seja necessário buscar um bean sem nome atribuído, podemos passar como parâmetro a class ou interface do objeto.

```java
Candidato bean = context.getBean(Candidato.class);
```
       
#### Buscando mais de um bean
       
Em situações que é necessário processar mais de um bean de uma mesma classe ou interface, podemos usar *getBeansOfType*, que retorna um mapa em que a chave é o identificador único de cada bean.

```java
Map<String, Candidato> beans = context.getBeansOfType(Candidato.class);
```

#### Inexistência do bean buscado
       
Quando o bean não existe, é lançado o erro *org.springframework.beans.BeansException* que pode ser aproveitado da seguinte forma:
       
```java
Candidato candidato = null;
       
try {
       candidato = context.getBean("candidato");
} catch (org.springframework.beans.BeansException ex) {
       candidato = new Candidato();       
}
```
       
Dado que os erros de instanciação, injeção de dependência e gerência de ciclo de vida serem normalmente fatais, é interessante no catch instanciar um bean de emergência. No entanto, o erro de *BeansException* é uma exceção d etempo de execução, e portanto, não é necessário que toda obtenção de um bean esteja dentro de um bloco try-catch.

### O ciclo de vida
       
O funcionamento real do Spring é ainda mais rico e oferece uma gama de possibilidade maior que só configurar, instanciar o contair e executar chamadas de funções de lookup. Para compreender melhor as possibilidades temos que visualizar o ciclo de vida do container em si.

<div align="center">
  <img alt="Pixel-Art" width="50%" src="https://github.com/AlexAlbert-Dev/Learning-Spring/blob/2d8cfc5745cdc55283941e6f85f33d1e9387c404/cicloVidaBean.png"/>
</div>

Explicando a definição de bean (*BeanDefinition*):
É originado de cada declaração de bean, e é responsável por representar internamente como é definido a configuração do bean. Para facilitar, imagine que o *BeanDefinition* é uma receita que o Spring vai seguir no gerenciamento dos beans. Dessa forma, ele tem como papel dizer qual classe deve ser instanciada, quais atributos devem ser preenchidos, dependências a serem injetadas etc.
       
### Escopos
       
É responsabilidade do escopo definir quando um bean deve ser instanciado, destruído e de quantas instâncias podem ser criadas. Existem diferentes escopos para tornar o Spring extremamente flexível, e para escolher entre as opções é interessante refletir sobre os seguintes pontos:
       
* Quanto demanda construir o bean: existem beans com maior demanda computacional para ser construídos, como um pool de conexões ou um EntityManager JPA. Devido a sua alta demanda computacional, é preferível que eles sejam construídos uma única vez durante a execução do sistema e não toda vez que forem demandados.

* O estado interno do objeto é compartilhável entre mais de um usuário do sistema: é necessário refletir sobre a necessidade ou não se o estado do objeto precisa ser único para cada usuário, e quais consequências poderia ter caso o estado fosse compartilhado.
       
* Tempo necessário de existência do objeto: as vezes não é interessante que alguns objetos durem muito, mesmo objetos que demandem muito poder computacional para serem construídos.
       
#### Singleton
       
Esse é o escopo padrão do Spring. Sua declaração pode ser feita da seguinte forma:

```xml
<bean id="bean" class="br.com.exemplo.ClasseExemplo" scope="singleton"/>
      <property name="nomePropriedade" value="/caminho/arquivo.csv"/>
</bean>
```
       
Suas características são:
       
* Apenas uma instância do bean será instanciada pelo container;
* A instância será compartilhada por todos os outros beans que dependam dela.
       
Esse escopo é recomendado nas seguintes situações:
       
* Objetos que demandam muito para serem construídos;
* Estado de objeto pode ser compartilhado sem gerar problemas ou caso seja positivo o compartilhamento entre mais de um objeto.
       
#### Prototype
       
Sua declaração pode ser feita da seguinte forma:
       
```xml
<bean id="bean" class="br.com.exemplo.ClasseExemplo" scope="prototype"/>
      <property name="nomePropriedade" value="/caminho/arquivo.csv"/>
</bean>
```

Suas características são:
 
* Garantia de que a cada requisição por um bean uma nova instância será criada.

Esse escopo é recomendado nas seguintes situações:

* Objetos que demandam pouco para serem construídos;
* Estado de objeto não compartilhável;
* Objeto útil por pouco tempo.
       
#### Request e Session
       
Sua declaração pode ser feita da seguinte forma:
       
```xml
<bean id="bean" class="br.com.exemplo.ClasseExemplo" scope="session"/>
      <property name="nomePropriedade" value="/caminho/arquivo.csv"/>
</bean>
       
<bean id="bean" class="br.com.exemplo.ClasseExemplo" scope="request"/>
      <property name="nomePropriedade" value="/caminho/arquivo.csv"/>
</bean>
```
       
Suas características são:
       
* O *request* mantém a existência da instância do bean enquanto a requisição existir;
* O *session* mantém a existência da instância do bean enquanto a sessão do usuário existir;

Esse escopo é recomendado nas seguintes situações:
       
* Evitar problemas de acesso concorrente no nosso código.

#### Customizados
       
O Spring possibilita a criação de escopos personalizados, para isso basta criar uma implementação da interface *org.springframework.beans.factory.config.Scope*. Sendo seu uso muito útil em situações que é necessário manter os beans somente enquanto algum tipo de conversão esteja ocorrendo etc. Um bom exemplo de sua utilidade é em situações que é necessário realizar a integração com sistemas remotos.

### Instanciação tardia
       
Por padrão as implementações do ApplicationContext inicializam todos os beans com escopo *singleton* quando sua configuração é iniciada. É isso que permite que caso ocorra algum problema, o desenvolvedor é notificado imediatamente. Mas em produção as vezes não é necessário ou benéfico que o bean seja inicializado junto do sistema, e nessas situações que faz sentido adiar a inicialização do bean para quando ele for necessário. Um efeito direto da instanciação tardia é a diminuição de tempo de boot do sistema e menos uso de memória.

A equipe do Spring chama esses beans com instanciação tardia de *lazy-initialized beans* e existe duas formas de defini-los no arquivo xml:
       
1. Tag ```<bean>``` :

```xml
<bean id="bean" class="br.com.exemplo.ClasseExemplo" lazy-init="true"/>
      <property name="nomePropriedade" value="/caminho/arquivo.csv"/>
</bean>
```

2. Padrão para todos os beans definidos no xml específico:

```xml
<beans xmlns="..." xmlns:xsi="..." xsi:schemaLocation="..."
       default-lazy-init="true">
       ...
</beans>
```


### Trabalhando com o ciclo de vida do Bean
       
Todo bean possui dois métodos de callback que são executado pelo seu container logo após sua inicialização e antes de sua destruição. A seguir vou discorrer sobre ambos os métodos.

#### Inicialização do bean
       
Existem duas maneiras de se trabalhar com o método de callback da inicialização do bean:

1. Implementando a interface *org.springframework.beans.factory.initializingBean*:
       
```java
import org.springframework.beans.factory.InitializingBean;
public class Exemplo implements InitializingBean { 
      public void afterPropertiesSet() throws Exception {...} 
       ... 
}
```
2. Adicionar o atributo *init-method* a tag ```<bean>``` (recomendado, pois o código continua completamente desacoplado do Spring):
       
```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo" init-method="metodoInit"/>
```

Observação: na forma 2 é necessário que o método seja do tipo *void* e não pode receber nenhum argumento como valor.
       
É possível padronizar o comportamento dos beans utilizando o atributo *default-init-method* à tag ```<beans>```, no entanto, um atributo *init-method* de uma tag ```<bean>``` sobreescreve o comportamento padrão. Implementação a seguir:

```xml
<beans xmlns="..." xmlns:xsi="..." xsi:schemaLocation="..."
       default-init-method="metodoInit">
</beans>
```

#### Finalização do bean
       
Existem duas maneiras de se trabalhar com o método de callback de finalização do bean:
       
1. Implementando a interface *org.springframework.beans.factory.DisposableBean*:
       
```java
import org.springframework.beans.factory.DisposableBean;
public class Exemplo implements DisposableBean { 
      public void destroy() throws Exception {...} 
       ... 
}
```
       
2. Adicionar o atributo *destroy-method* a tag ```<bean>``` (recomendado, pois o código continua completamente desacoplado do Spring):
       
```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo" destroy-method="metodoDestroy"/>
```
       
Observação: na forma 2 é necessário que o método seja do tipo *void* e não pode receber nenhum argumento como valor.
       
É possível padronizar o comportamento dos beans utilizando o atributo *default-destroy-method* à tag ```<beans>```, no entanto, um atributo *destroy-method* de uma tag ```<bean>``` sobreescreve o comportamento padrão. Implementação a seguir:

```xml
<beans xmlns="..." xmlns:xsi="..." xsi:schemaLocation="..."
       default-destroy-method="metodoDestroy">
</beans>
```
       

### Fazendo Bean conhecer seu container
       
Para um bean se tornar consciente com seu container basta que o mesmo implemente a interface *aware*, existindo uma própria para container do tipo ApplicationContext e do tipo BeanFactory. Como benefício do bean conhecer seu container é evitar situações que geram um singleton acidental, ou seja, onde um bean singleton utiliza na tag ```<property>``` um bean prototype, isso ocorre porque todo bean pertencente ao escopo *singleton* é instanciado uma vez.

Como exemplo, vamos ver uma classe singleton que utiliza um bean prototype em seu escopo, sendo necessário gerar uma nova instância desse bean prototype a cada requisição. A seguir um xml que ocasiona o singleton acidental:

```xml   
<bean id="beanPrototype" class="br.com.exemplo.ClasseBeanPrototype" scope="prototype"/>
       
<bean id="exemplo" class="br.com.exemplo.Exemplo" scope="singleton">
       <property name="impressor" ref="impressor"/>
</bean>
``` 

A implementação que faz o bean prototype conhecer seu container e resolve o problema de singleton acidental:
       
```java
public class Exemplo implements ApplicationContextAware {
       private ApplicationContext context;
       
       public void setApplicationContext(ApplicationContext ap) throws BeansException {
              this.context = ap;
       }

       public ClasseObjeto getObjeto() {
              return context.getBean(ClasseObjeto.class);
       }
}
```

### Modularização das configurações
       
Podemos modularizar nossos arquivos de configuração, permitindo organizar os beans de maneira lógica visando a manutenção do sistema. Dado isso, é comum separarmos as configurações dos nossos beans em arquivos de configuração xml distintos. O Spring visando auxiliar nesse processo, possibilita em todas as suas implementações de ApplicationContext um construtor que recebe como parâmetro um array de strings indicando a localização dos arquivos de configuração. 

A seguir um exemplo do construtor com os parâmetros:

```java
ApplicationContext context = new ClassPathXmlApplicationContext({"config1.xml","config2.xml"});
```
       
Além disso, é possível que um arquivo de configuração importe outro, através da tag ```<import>```. A seguir um exemplo: 

```xml
<import resource="config1.xml"/>

<bean id="exemplo" class="br.com.exemplo.ClasseExemplo"/>
...
```
       
### Aplicação de herança
       
Para aplicar o conceito de herança nos nossos arquivos de configuração, e consequentemente diminuir o texto digitado, um bean carrega a configuração e os outros beans que forem utilizar essa configuração apenas herdam ela através da tag ```<parent>```. Um exemplo a seguir:

```xml
...
<bean id="beanPai" class="br.com.exemplo.ClasseBeanPai">
       <property name="beanUtilizado1" ref="idBeanUtilizado1"/>
       <property name="beanUtilizado2" ref="idBeanUtilizado2"/>
</bean>

<bean id="beanFilho" parent="beanPai">
       <property name="beanUtilizado1" ref="idOutroBean"/>
</bean>
```
       
O bean filho sobreescreve qual bean será o "beanUtilizado1" mas mantém o restante das configurações do bean pai.

</details>


<div id='springel'/>

## 11. Spring Expression Language (SpEL)

<details>

<summary> Conteúdo: </summary>
       
O Spring Expression Language permite que os valores de atributos e dependências sejam conhecidos apenas em tempo de execução, adicionando assim um dinamismo as configurações. Permite injetar valores via setter e via construtores nos beans, baseado em expressões. Essa linguagem se assemelha com a Expression Language vista no JSP.

### Sintaxe básica

O conteúdo que estiver entre ```#{``` e ```}``` deve retornar um valor, inclusive um literal. Um exemplo de literal abaixo:

```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo">
       <property name="nomePropriedade" value="#{4}"/>  
</bean>
```
       
Outros exemplos de literal:

```xml
#{3.14}
'#{"texto com aspas duplas"}'
#{false}
```

### Operações básicas e comparação
       
Todas as operações de Java estão disponíveis.

#### Operações aritméticas
       
Os operadores aritméticos disponíveis: soma (+), subtração (-), multiplicação (*), divisão (/), módulo (%) e potenciaçãp (^). Um exemplo de operador aritmético abaixo: 
       
```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo">
       <property name="nomePropriedade" value="#{4 * 2}"/>  
</bean>
```

#### Operadores relacionais

Os operadores relacionais disponíveis: menor que (lt), maior que (gt), igual (eq), diferente (ne), maior ou igual (ge) e menor ou igual (le). Sempre retornar um valor booleano. Um exemplo de operador relacional abaixo:

```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo">
       <property name="nomePropriedade" value="#{1 gt 2}"/>  
</bean>
```

#### Operadores lógicos

Os operadores lógicos disponíveis: and, or e not. Retornar sempre um valor booleano. Um exemplo de operador lógico abaixo:

```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo">
       <property name="nomePropriedade" value="#{not ((1 eq 2) or (2 ne 4))}"/>  
</bean>
```

#### Operador ternário

Assim como em Java, existe o operador ternário que se baseia da seguinte forma: ```(expressão) ? (retorno caso verdadeiro) : (retorno caso falso)```. Um exemplo de operador ternário abaixo:

```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo">
       <property name="nomePropriedade" value="#{2 eq 4 ? 2 : 4}"/>  
</bean>
```

### Referência a outros beans

O SpEL permite acessar atributos presentes em outros beans, e é aqui que está a verdadeira utilidade do mesmo. Para isso a sintaxe utilizada é a seguinte: ```(identificador do bean).(nome de atributo/propriedade)```, no entanto o nome de atributo/propriedade é optativo. Um exemplo de seu uso abaixo:

```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo">
       <property name="beanInjetado" value="#{idClasseDoBeanInjetado.atributo}"/>  
       <property name="beanInjetado2" value="#{idClasseDoBeanInjetado2.atributo?.metodo()}"/>  
</bean>
```

Observação: o operador ```?``` é de nulidade, ou seja, não lança uma exceção do tipo NullPointerException caso o atributo seja nulo.

### Propriedades do ambiente de execução

Além dessas possibilidades, o SpEL permite a obtenção da situação do ambiente de execução. Para tal, é utilizado o objeto *systemProperties* e corresponde a uma chamada *getProperties()* da classe java.lang.System. 

Como exemplo, vamos utilizar um bean que busca os arquivos dentro do diretório *documentos* do usuário corrente:

```xml
<bean id="fonteDados" class="br.com.exemplo.FonteDados">
       <property name="arquivo" value="#{systemProperties['user.documento']}/arquivo.csv"/>
</bean>
```
       
</details>

<div id='autowiring'/>

## 12. Autowiring

<details>

<summary> Conteúdo: </summary>

### Introdução

Visando aumentar a produtividade, podemos configurar o container através do autowiring. O autowiring permite automatizar a injeção de dependência, retirando a necessidade de configuração manual da mesma.

Cabe ressaltar que o uso do autowiring, sem o uso de anotações, é recomendado somente em projetos pequenos ou projetos com convenções rígidas.

### Autowiring

Se trata da injeção automática de dependendências, possibilitando que o container descubra em tempo de execução, sem necessidade de instrução do desenvolvedor, quais dependências devem ser injetadas em cada bean.

#### Autowiring no XML

Um exemplo de uso de autowiring em todos os beans declarados em um arquivo XML:

```xml
<beans default-autowire="byType" xmlns="..." xmlns:xsi="..." xsi:schemaLocation="...">
       <bean class="br.com.exemplo.Exemplo">
              <property name="metodo" value="parametro"/>
       </bean>
       
       <bean class="br.com.exemplo.ClasseExemplo1"/>
       <bean class="br.com.exemplo.ClasseExemplo2"/>
       <bean class="br.com.exemplo.ClasseExemplo3"/>
</beans>
```

Observação: dado que o container que descobre as dependências, não é necessário nomear os nossos beans.

#### Valores do autowire

Os valores que o autowire aceita são:

* no : este é o valor padrão do spring, e significa que o autowiring não será adotado;
* byType : uma dependência será injetada caso seja encontrada APENAS uma definição do bean que possua um tipo compatível;
* byName : uma dependência será injetada baseado no nome, ou seja, serão buscados beans que tenham o mesmo nome que a propriedade que receberá a injeção;
* constructor : o container irá buscar por construtores não padrão para executar a injeção em vez de propriedades.
* autodetect : o container tenta injetar através de uma injeção do tipo construtor, caso não seja encontrado um construtor no bean, será adotada a injeção byType.

### Ambiguidade

Como esperado, um dos maiores problemas encontrados no autowiring é a ambiguidade. É fundamental que não exista ambiguidade para que a injeção automática funcione corretamente.

A maneira mais eficaz para solucionar a ambiguidade é instruir o container quais beans não devem ser injetados automaticamente. Para instruir ao container o bean que deve ser ignorado na injeção automatica é necessário igual o exemplo abaixo:

```xml
<bean id="exemplo" class="br.com.exemplo.ClasseExemplo" autowire-candidate="false">
       <property ... >
</bean>
```

### Vantagens do autowire

Como principais vantagens do autowire podemos destacar:

* Redução da configuração;
* Evolução natural e automática da nossa configuração;


### Limitações do autowire

Como principais limitações do autowire podemos destacar:

* Troca do explícito pelo implícito, pois dellegamos ao container como deve ser feita a injeção de dependências;
* Injeção automática de propriedades primitivas, listas e strings. É uma desvantagem pois é necessário que o desenvolvedor defina seus valores explicitamente sempre.

</details>

<div id='anotacoes'/>

## 13. Anotações do Spring

<details>

<summary> Conteúdo: </summary>

</details>

## Minhas certificações sobre o assunto
[Java Servlet: programação web Java]()
