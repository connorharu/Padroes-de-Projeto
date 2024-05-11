# Padroes-de-Projeto
Atividadade sobre Padrões de Projeto da aula de Engenharia de Software.
## Creational Patterns - Builder
O padrão de criação Builder é utilizado para situações onde é necessária a construção de objetos complexos passo-a-passo. O objeto em questão requer uma construção elaborada, ou segue uma lógica específica. Ele permite que você tenha diferentes tipos de representação de um objeto, mantendo o mesmo código de construção.
### Caracterização do problema
A inicialização de um objeto complexo pode possuir vários parâmetros, fazendo com que seu construtor fique complexo demais. Portanto, o código pode se tornar desorganizado e difícil de entender - a lógica de construção ficaria dispersa pelo código, complicando a manutenção. Além disso, certas linhas de código podem acabar sendo repetidas sempre que for necessário construir objetos semelhantes em diferentes partes do sistema, tornando a reutilização de código menor.
### Solução para o problema
O código que construi o objeto pode ser removido de sua classe e separada em objetos chamados _builders_. Por exemplo, em um sistema onde o cliente deseja montar carros, nós teremos a classe _carro_ e os _builders_ que poderão construir: as portas, as rodas, as janelas, o motor..., criando um objeto executando esses procedimentos passo-a-passo. O principal ponto desse método é que você não precisa chamar todos os _builders_, e sim aqueles que são necessários para o objeto que você precisa.

Para lidar com as possíveis diferentes demandas de construção, como por exemplo um carro blindado ou um ônibus, podemos ter várias classes de construtor, cada uma implementando os mesmos passos, mas de maneiras diferentes. Assim, teremos várias versões do objeto final. No entanto, isso somente funcionaria se o código do cliente que chama os métodos de construção possui habilidade de interação com os construtores, utilizando de uma interface em comum.

Adicionalmente, esses passos para construir um certo objeto podem ser extraídos e colocados em uma classe separada chamada _director_. Ela irá definir a ordem em que esses passos serão executados, enquanto o builder lhe dá a implementação para esses passos. Não é uma classe obrigatória, porém, é fortemente recomendável.
![structure](https://github.com/connorharu/Padroes-de-Projeto/assets/142368559/bf18dbe4-227b-4dfe-8fc3-e208ac7b176e)
