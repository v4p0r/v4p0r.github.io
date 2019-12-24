---
layout: post
title: Consultando dados públicos com Perl
---

## Consultando dados públicos com Perl

Estava um dia navegando pela maravilhosa internet, quando me deparo com um site governamental que entre aspas, oferecia uma consutória de CPF, bom não fui mais a fundo então não posso dizer se isso é uma falha ou algo que realmente poderia ser utilizado.

Fiquei curioso e claro, como todo curioso resolvi futricar e assim veio uma ideia e acabei criando um script simples no qual você pode fazer consultas utilizando essa "API". Então depois dessa ideia do script me veio a cabeça escrever um pouco mais sobre, então escrevi, bom vou parar de enrolação aqui, e deixar vocês continuarem e descobrirem como tudo foi criado hehe.

### Introdução

Eae, tudo certo? Bom espero que sim hehe. Hoje trago aqui para vocês um jeito simples de consultar dados públicos de um CPF. Então vamos lá, lembrando que não me resposabilizo pelo atos de quem fizer fruto desse artigo.

### Materiais

Para realizar alguns procedimentos do artigo é necessário que você tenha um interpretador
da linguagem Perl, a API de consulta e os modúlos LWP::Simple e XML::Simple. Se você já tiver isso em sua máquina vamos lá, se não instala ae rapidim chapa e vamos.

### Let's go

Primeiro de tudo vamos declarar os modúlos que serão usados, LWP::Simple e XML::Simple.

```perl
#!/usr/bin/perl
 
# Um pouco de Perl e dados Públicos
# 22 FEV 2018
 
use warnings;
use strict;
use LWP::Simple;
use XML::Simple;
 
my $parser = new XML::Simple;
```

Você deve ter reparado que ali tem outros dois modúlos o warnings e o strict, de uma forma
mais rápida para você entender aqueles modúlos ajudam você a deixar seu código mais legível.
 
Bom agora vamos acessar a API. [**link**](http://www.juventudeweb.mte.gov.br/pnpepesquisas.asp?acao=consultar+cpf&cpf=), como podem ver essa API retorna um arquivo em
XML então precisamos interpretar esse XML no Perl, e para isso utilizamos o modúlo
XML::Simple, tem outros mas vamos utilizar ele hoje. Para isso criamos uma várivel e atribuiamos a ela o valor da função XML::Simple

```perl
#!/usr/bin/perl
 
# Um pouco de Perl e dados Públicos
# 22 FEV 2018
 
use warnings;
use strict;
use LWP::Simple;
use XML::Simple;
 
my $parser = new XML::Simple;
```

**Espero que esteja entendendo tudo certinho, me desculpe por alguns erros, é meu primeiro artigo haha. Mas continuando, agora é a hora de pegar as informações certo? Então vamos.**
Vamos criar primeiro uma várivel que armazene nosso CPF a ser consultado e uma que armazene nossa url, para que tudo fique bem organizado.

```perl
my $url = 'http://www.juventudeweb.mte.gov.br/pnpepesquisas.asp?acao=consultar+cpf&cpf='.$cpf;
```

Organizadinho e bonitinho, ta na hora de fazer a requisição e puxar logodados, chega de enrolar e bora pegar essas informações. Para que possamos fazer isso vamo usafunção GET, do modúlo LWP::Simple e logo em seguida interpretar o XML do retorno.

```perl
my $content = get($url) or die "Erro de conexao $url\n";
my $data = $parser->XMLin($content);
```

Bom já fizemos o GET, já interpreamos o XML, agora é só puxar os dados que nos convem,podemos fazer isso da seguinte forma.

```perl
print "\n[!] NOME: ".$data->{PESSOAFISICA}{NOPESSOAFISICA}. "\n".
        "[!] CPF: ".$data->{PESSOAFISICA}{NRCPF}. "\n".
        "[!] DATA DE NASCIMENTO: ".$data->{PESSOAFISICA}{DTNASCIMENTO}. "\n".
        "[!] CEP: ".$data->{PESSOAFISICA}{NRCEP}. "\n".
        "[!] CIDADE: ".$data->{PESSOAFISICA}{NOMUNICIPIO}."-".$data->{PESSOAFISICA}{SGUF}. "\n".
        "[!] BAIRRO: ".$data->{PESSOAFISICA}{NOBAIRRO}. "\n".
        "[!] NOME DA MAE: ".$data->{PESSOAFISICA}{NOMAE}. "\n";
```

E pronto, consulta de dados simples utilizando Perl. Claro esse é um script do mais simples criado, você pode melhorar e muito esse script, isso só foi uma base. 

### Conclusão

Esse artigo foi criado com intuito de demonstrar uma consulta de dados públicos usando a linguagem Perl, aprendemos por cima tambem um pouco sobre parsear XML e fazer requisição GET utilizado com LWP.
 
Esse artigo foi o primeiro que escrevi como disse logo acima, pode conter erros de ortografia, entre outros mas nada que eu não possa modificar com o tempo.

### Referências
- http://search.cpan.org/~grantm/XML-Simple-2.24/lib/XML/Simple.pm
- http://search.cpan.org/~ether/libwww-perl-6.15/lib/LWP/Simple.pm
