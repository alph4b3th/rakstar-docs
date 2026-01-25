# Introdução

## Sobre

RakStar é um framework moderno e opinativo open-source para a linguagem de programação Go feito para e por aqueles que desejam um ambiente agradável, produtivo e elegante para o desenvolvimento de servidores performáticos e modernos.

## Um breve resumo

### Problemas do Pawn

Pawn é uma linguagem estruturada simples e limitada, tendo poucos recursos para desenvolvimento de sistemas complexos, por mais que o ecossistema envolva diversas bibliotecas feitas pela comunidade. Fazer um GameMode do zero em Pawn implicará diversos problemas ao longo do caminho, sendo o maior deles a produtividade.  

A falta de estrutura de dados simples como `struct` e `map` é lamentável. A necessidade de utilização de arrays como uma gambiarra para struct é um atraso significativo que agrava ainda mais a legibilidade do código. É necessário a criação de enums, que não possuem isolamento como no C. Como consequência, os nomes dos membros dos enumeradores e das tags se tornam cada vez maiores, sujando o código.

Desenvolver um GameMode em Pawn significa, majoritariamente, lidar com um ambiente single-thread. Para evitar o bloqueio da execução principal, o que causaria o congelamento do servidor, somos obrigados a confiar tarefas pesadas em callbacks de plugins externos. Essa abordagem traz severos riscos estruturais, pois ao utilizarmos essa estratégia, perdemos o controle sobre o fluxo de execução e introduzimos a possibilidade de condições de corrida ([race conditions](https://en.wikipedia.org/wiki/Race_condition)). 

O ambiente de desenvolvimento é confuso e limitado. Pawn não possui [LSP](https://en.wikipedia.org/wiki/Language_Server_Protocol), não tem ferramentas de teste nativas, o compilador é lento, modularização, embora possível, é precária.   

Conforme o seu GameMode cresce, mais problemas surgem. A legibilidade decai (aumentando o surgimento de bugs), você se sente sobrecarregado com tantas bibliotecas com padrões diferentes para resolverem problemas simples, e o pior de tudo, o código do seu GameMode estará inchado. Embora a simplicidade seja boa, no caso do Pawn ela atrapalha a produtividade.  

O que a comunidade fez pelo SA-MP é incrível, com todas as bibliotecas e plugins feitos com cuidado. No entanto, muitas dessas bibliotecas e plugins não são mantidas e já estão paradas no tempo. Infelizmente, ainda sentimos falta de diversas bibliotecas novas e soluções modernas que surgem em outras linguagens.

### Golang, a solução?

Go é uma linguagem moderna, performática, simples, segura e de fácil aprendizagem; uma tecnologia que se casa perfeitamente com o desenvolvimento de sistemas complexos. Possui um ecossistema rico e ferramentas poderosas para solucionar problemas de maneira produtiva.

[Goroutine](https://go.dev/tour/concurrency/1) é uma implementação de [green threads](https://en.wikipedia.org/wiki/Green_thread) extremamente eficiente e cativante, que possui a mais fácil usabilidade dentre outras linguagens. Go também oferece um sistema de [mutex](https://go.dev/tour/concurrency/9) sólido e simples. Uma comunicação de [canais](https://go.dev/tour/concurrency/2) agradável com "[syntatic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar)". Com as ferramentas de concorrência do Go, você terá uma forma sólida de aumentar a performance do GameMode de maneira legível, confiável e escalável.

Testes em Go são extremamente fáceis de criar. Go possui um ferramental completo para você testar a execução do seu código, seja com testes unitários, integração e de benchmark. Com o framework de testes do Go, seu GameMode terá muito mais consistência e reduzirá consideravelmente a quantidade de bugs, garantindo mais tempo para focar em novos sistemas sem quebras surpresas.

Ambiente de desenvolvimento completo com todas as ferramentas de última geração. Um gerenciador de pacotes estável, um compilador rápido e confiável, sistema de modularização avançada e intuitiva. Go possui milhares de bibliotecas e ferramentas mantidas veementemente pela comunidade, como [ORMs](https://pt.wikipedia.org/wiki/Mapeamento_objeto-relacional) e Drivers para diversos Bancos de Dados, inteligência artificial, implementações de algoritmos, conectores para Discord, Telegram e Whatsapp, isso tudo é só um pedaço do que você poderá encontrar. Você poderá usar grande parte disso e ter um GameMode completo, fácil de desenvolver. 

O céu é o limite. Com criatividade, você será capaz de construir qualquer GameMode produtivamente e sem muitas restrições com todo o poder que o Go pode lhe oferecer. Caso não saiba programar em Go, sem problemas. Go é uma das linguagens com a menor curva de aprendizagem, leia a [documentação](https://go.dev/doc/). 

### Rakstar

O Rakstar é um framework completo escrito em Go para desenvolvimento de GameModes. Buscamos implementar funcionalidades complexas com utilizações simples. Você poderá fazer qualquer GameMode que imaginar escrevendo pouco código, mantendo a mais alta legibilidade possível ([leia este artigo](https://alph4b3th.medium.com/rakstar-o-maior-framework-para-samp-87196745109b)).

Deseja fazer um comando? Não se preocupe com verificações bobas, o Rakstar fará isso por você. Ao longo da documentação, você aprenderá diversos recursos que facilitarão a sua vida, e descobrirá como construir um servidor SA-MP funcional. Essa ferramenta pode gerenciar todas as necessidades do seu projeto com riqueza de recursos.

Por ser um framework opinativo, você seguirá os padrões pré-determinados, ganhando consistência e manutenibilidade para os seus projetos.


