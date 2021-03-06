# Fonte Tensão 12V
Projeto de uma Fonte de Tensão Variável de 3V a 12V, desenvolvido para a disciplina de Eletrônica para Computação, ministrada pelo Professor Dr. Eduardo do Valle Simões
# Circuito Falstad
<a href = "http://tinyurl.com/yclddzmd"> Circuito Falstad</a>
![Falstad](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/FonteFalstad.png)
# Circuito EAGLE

![PCB](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/PCB.png)

<a href = "https://drive.google.com/file/d/1tgSCFVqVofuXs6VpP3hKhitnwWkWix8T/view?usp=sharing"> Vídeo Explicativo</a>

# Esquemático EAGLE
![Schematic](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Schematic.png)

## Conversor Corrente Alternada de Alta Tensão para Corrente Alternada de Baixa Tensão
Ao usar um transformador de 127V AC para 24V AC, com a razão descrita abaixo, a tensão do circuito, fica adequada aos componentes usados na fonte, que são mais sensíveis a valores altos de tensão e suas variações. Quando há variação de fluxo na corrente primária (127V), na parte secundária, é gerada uma corrente induzida com um potencial elétrico menor (24V). O intervalo de oscilação sai de 127V a -127V para 24V a -24V. A razão obtida na fórmula abaixo, auxiliou a resolução da problemática no Falstad, para conseguir ajustar a tensão secundária do transformador.

![Transformador](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Transformador.gif)

## Conversor Corrente Alternada para Corrente Contínua
Como estamos lidando com uma corrente alternada (24V a -24V), é necessário uma estratégia para converter desse tipo de corrente elétrica para contínua, para o funcionamento adequado da fonte. Assim, usa-se uma ponte retificadora feita com diodos, cuja função é tornar a corrente unidirecional devido a propriedade do diodo, que permite o fluxo de corrente em apenas um sentido determinado. Por motivos de incompatibilidade com o Eagle e indisponibilidade no mercado, mas ainda avaliando a proporcionalidade do diodo/PCB, escolhemos a ponte retificadora KBPC1010 pois ela é compatível com o Eagle e está disponível na loja escolhida. 
Assim, a tensão na saída da ponte retificadora será uma unidirecional, porém instável para se usar dessa maneira na saída. Como a corrente nominal é, no máximo, de 1.205 A, usamos uma ponte retificadora de diodos que resiste a uma corrente de até 10 A, maior que o necessário, porém foi a ponte com menor especificação que encontramos na distribuidora.

## Filtragem de Corrente
A saída da ponte retificadora é instável, possuindo pequenas oscilações em sua voltagem, dessa forma forma será usado um capacitor conectado em paralelo com o output, com o objetivo de diminuir essas oscilações para a próxima parte do circuito. O objetivo de colcoar um capacitor, se baseia no fato do circuito precisar de tensão/corrente durante a parte na qual os valores de tais atributos estão baixos, necessitando que o capacitor se descarregue para manter uma margem confortável para o funcionamento geral do circuito.
O capacitor ao carregar e descarregar durante seu ciclo fornece um aumento linear da voltagem, filtrando o sinal e disponibilizando uma tensão mais estável. Dessa forma, escolhemos um capacitor de 470uF, valor ideal para manter uma corrente sem muitas oscilações.

![Capacitor](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Capacitor.gif)

## Regulador de Tensão
O Diodo Zener associado ao potenciômetro, conjuntamente com o Transistor NPN, permite que a tensão e corrente seja regulada e programada a partir de sua resistência variável.
- <b>Diodo Zener:</b> Ao ser conectado em paralelo, será responsável pela regulação da tensão máxima que será fornecida ao Potenciômetro. Esse componente atua filtrando a tensão somente quando for fornecido a ele uma tensão suficientemente maior do que a sua tensão nominal, 13V no caso deste projeto. Dessa forma, quando o Diodo Zener estiver ativo parte da corrente do circuito será absorvida e uma tensão 13V estável será fornecida ao Transistor. O Resistor ![R2value](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Formulas/R2val.gif) conectado será responsável por limitar a corrente que flui em direção ao Zener, tornando seguro o uso do modelo escolhido. Nota-se pelo simulador que há um pico de corrente de, aproximadamente, -1.5mA e que o Diodo está submetido a uma tensão de -13V, dissipando no máximo 19.5mW. Dessa forma, o componente escolhido (com especificações para 500mW) será suficiente.

- <b>Potenciômetro:</b> Resistor variável que permitirá regular a tensão elétrica de saída. Em conjunto com restante do circuito, o Potenciômetro foi devidamente projetado para fornecer uma tensão final no intervalo entre 3V e 12V, atendendo as especificações do projeto. Quando ajustado para uma resistência mínima, a tensão de saída será aproximadamente 12V. Analogamente, quando for regulado em sua resistência máxima a tensão fornecida será próxima de 3V. O Resistor ![R4](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Formulas/R4val.gif) será responsável por regular a tensão fornecida ao transistor, adequando-a a 3V, quando o potenciômetro estiver com resistência máxima.

- <b>Transistor NPN:</b> Quando sobre ele é aplicada uma voltagem em sua base o Transistor passa a atuar em seu estado condutivo. Tem como papel, nesse circuito, amplificar a corrente, fornecendo ao circuito uma corrente, através do coletor, na ordem de 100 vezes maior do que a corrente base. Para atender ao requisitos do projeto (Fonte de Tensão com no máximo 100mA) foi escolhido um resistor de ![R120](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Formulas/120R.gif) para que a corrente final do circuito, quando regulado para 12V, permaneça nesse valor. Nota-se então que a corrente de base, e a corrente final possuirão os seguintes valores nos extremos:
  - ![Vout12](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Formulas/Vout_12.gif)
  - ![Vout3](https://github.com/Alvaro-dev/FonteTensaoVariavel/blob/master/Formulas/Vout_3.gif)  

Vale notar que há no Transistor uma queda de tensão de aproximadamente 715 mV quando a tensão de base for 13V e de 678 mV quando a tensão da base for, aproximadamente, 4.6V. É preciso considerar essa queda de tensão uma vez que a tensão final do circuito será o resultado da tensão de base subtraída da queda de tensão no Transistor.

# Orçamento dos materiais usados
Tabela com os preços dos componentes
|     Nome    |     Especificações    |     Preço    |
| :-------------: | :----------: | :-----------: |
| Transformador | Transformador Trafo 12V+12V 500mA – 110/220VAC | <a href = "https://www.baudaeletronica.com.br/transformador-trafo-12v-12v-500ma-110-220vac.html">R$ 22,23</a>|
| Resistor 1.5K   | Resistor 1.5K 5% (1/4W) | <a href = "https://www.baudaeletronica.com.br/resistor-1k5-5-1-4w.html">R$ 0,08</a> |
| Resistor 2.7K   | Resistor 2.7K 5% (1/4W) | <a href = "https://www.baudaeletronica.com.br/resistor-2k7-5-1-4w.html">R$ 0,08</a> |
| Resistor 2.2K   | Resistor 2.2K 5% (1/4W) | <a href = "https://www.baudaeletronica.com.br/resistor-2k2-5-1-4w.html">R$ 0,08</a> |
| Resistor 120R   | Resistor 120R 5% (1/4W) | <a href = "https://www.baudaeletronica.com.br/resistor-120r-5-1-4w.html">R$ 0,08</a> |
| LED | Led Difuso 5mm Vermelho | <a href = "https://www.eletrogate.com/led-difuso-5mm-vermelho">R$ 0,25</a> |
| Diodo Zener   | Diodo Zener BZX55C [13V / 0.5W] | <a href = "https://www.baudaeletronica.com.br/diodo-zener-bzx55c-13v-0-5w.html">R$ 0,09</a> |
| Capacitor   | Capacitor Eletrolítico 470uF / 35V | <a href = "https://www.baudaeletronica.com.br/capacitor-eletrolitico-470uf-35v.html">R$ 0,57</a> |
| Potenciômetro 5k   | Potenciômetro Linear de 5K (5000Ω) | <a href = "https://www.baudaeletronica.com.br/potenciometro-linear-de-5k-5000.html">R$ 1,45</a> |
| Ponte Retificadora KBPC1010* | Ponte Retificadora de 10A | <a href = "https://www.baudaeletronica.com.br/ponte-retificadora-kbpc1010.html"> R$ 3,79  </a> |
| Transistor   | Transistor NPN 2N3904 | <a href = "https://www.baudaeletronica.com.br/transistor-npn-2n3904.html">R$ 0,17</a> |
| Total   |  | R$ 28,87 |

* Poderíamos ter usado componentes mais baratos, mas devido ao fato do software Eagle/Loja não possuirem os modelos modelos mais adequados, escolhemos esse.

# Agradecimentos
Queremos agradecer ao professor Dr Eduardo Simões pela oportunidade de fazer o trabalho com as retificações necessárias, conforme especificado. Esperamos que, no futuro, poderemos usufruir novamente de alguma disciplina ministrada pelo professor, em um contexto cujo aprendizado não seja à distância.

Queremos também agradecer aos criadores do website: https://componentsearchengine.com/ por nos disponibilizar as libraries do EAGLE de graça.

# Contribuição
Alvaro José Lopes - 10873365

Paulo Henrique de Souza Soares - 11884713
