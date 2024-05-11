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

_Texto ALT: diagrama UML sobre o padrão de criação Builder. Imagem retirada do site refactoring.guru_.

### Código de exemplo
```cpp
#include <iostream>
#include <string>
#include <stdlib.h>
using namespace std;

// carro
class Carro {
private:
    string modelo;
    string janelas;
    string rodas;
    string outrosDetalhes;
public:
    void setModelo(string modelo) {
        this->modelo = modelo;
    }
    void setJanelas(string janelas) {
        this->janelas = janelas;
    }
    void setRodas(string rodas) {
        this->rodas = rodas;
    }
    void setOutrosDetalhes(string outrosDetalhes) {
        this->outrosDetalhes = outrosDetalhes;
    }

    void mostrarDetalhes() {
        cout << "Modelo: " << modelo << endl;
        cout << "Janelas: " << janelas << endl;
        cout << "Rodas: " << rodas << endl;
        cout << "Outros detalhes: " << outrosDetalhes << endl;
    }
};

// builder
class CarroBuilder {
public:
    virtual void construirModelo() = 0;
    virtual void construirJanelas() = 0;
    virtual void construirRodas() = 0;
    virtual void construirOutrosDetalhes() = 0;
    virtual Carro* getCarro() = 0;
};

// builder de carro padrão
class CarroBuilderPadrao : public CarroBuilder { // herança
private:
    Carro* carro; // aponta pra um objeto do tipo Carro
public:
    CarroBuilderPadrao() {
        carro = new Carro(); // apontando para um novo carro
    }
    void construirModelo() override { // override está sobre-escrevendo a função da classe base
        carro->setModelo("Carro Padrão");
    }
    void construirJanelas() override {
        carro->setJanelas("Janelas comuns");
    }
    void construirRodas() override {
        carro->setRodas("Rodas comuns");
    }
    void construirOutrosDetalhes() override {
        carro->setOutrosDetalhes("Outros detalhes comuns");
    }
    Carro* getCarro() override {
        return carro;
    }
};

// builder de carro blindado
class CarroBuilderBlindado : public CarroBuilder {
private:
    Carro* carro; // aponta pra um objeto do tipo Carro
public:
    CarroBuilderBlindado() {
        carro = new Carro();  // apontando para um novo carro
    }
    void construirModelo() override {  // override está sobre-escrevendo a função da classe base
        carro->setModelo("Carro Blindado");
    }
    void construirJanelas() override {
        carro->setJanelas("Janelas blindadas");
    }
    void construirRodas() override {
        carro->setRodas("Rodas reforçadas");
    }
    void construirOutrosDetalhes() override {
        carro->setOutrosDetalhes("Sistema de segurança avançado");
    }
    Carro* getCarro() override {
        return carro;
    }
};

// diretor
class Diretor {
public:
    void construirCarro(CarroBuilder* builder) { // ponteiro builder que aponta para um objeto de tipo CarroBuilder,
        builder->construirModelo();              // método público da classe chamado construirCarro que utiliza das implementações
        builder->construirJanelas();             // fornecidas pelo builder, decidindo a ordem de execução dos passos.
        builder->construirRodas();
        builder->construirOutrosDetalhes();
    }
};

int main() {
    Diretor diretor;
    
    // carro padrão
    CarroBuilderPadrao builderPadrao;
    diretor.construirCarro(&builderPadrao); // construção de um carro utilizando builderPadrao
    Carro* carroPadrao = builderPadrao.getCarro(); // ponteiro para o carro padrão construído
    carroPadrao->mostrarDetalhes();
    
    cout << endl;
    
    // carro blindado
    CarroBuilderBlindado builderBlindado;
    diretor.construirCarro(&builderBlindado); // construção de um carro utilizando builderBlindado
    Carro* carroBlindado = builderBlindado.getCarro();  // ponteiro para o carro blindado construído
    carroBlindado->mostrarDetalhes();
    
    return 0;
}
```

## Structural Patterns - Decorator
O padrão _decorator_, ou _wrapper_, é um design de padrão estrutural utilizado para acoplar novos comportamentos em objetos, colocando estes mesmos objetos dentro de um invólucro especial que contenha esses comportamentos.
### Caracterização do problema
Com as possíveis mudanças de uma classe e implementação de novas necessidades, há uma grande chance de que o código se torne _bloated_. Portanto, torna-se necessária outra maneira de organizar o código, a fim de que não exista código duplicado, dificuldade na composição das fucnionalidades específicas de objetos ou que o princípio _open/closed_ seja violado para modificação da classe base, facilitando a manutenção do código.
### Solução para o problema
Deve-se priorizar o uso de agregação ou composição ao invés de herança. Na agregação, o objeto teria uma referencia para uma outra e encarregaria algumas de suas funções à ela, e na composição, o objeto conseguiria fazer esse trabalho sozinho, herdando o comportamento da sua superclasse. Com essa solução, se torna possível substituir o objeto de suporte vinculado assim que necessário, alterando o comportamento em tempo de execução. Dessa maneira, um objeto teria acesso ao comportamento de várias classes, referências à múltiplos objetos e poderia encarregá-los com suas funções.

A função de suporte seria o _wrapper_, um padrão do decorador que deixa explícito o uso do padrão. Ele pode ser vinculado com um objeto específico. O embrulho possui todos os métodos que o objeto específico, encarregando esse objeto com todas as suas funções. Portanto, para o cliente, eles são idênticos. No entanto, ele pode realizar ações antes ou depois de entregar o pedido para o objeto, similarmente com um construtor/destrutor dentro de uma classe. Pense como se fosse uma boneca russa: um_ wrapper_ dentro de outro _wrapper_ dentro de outro..., e no fundo, o objeto em específico. O _wrapper_ aceitará qualquer objeto que siga a mesma interface do objeto encapsulado, permitindo adicionar _wrappers_ com funcionalidades adicionais de forma dinâmica e flexível.

Por exemplo, pense em um café preto comum: esse será o objeto que encapsularemos. Podemos colocar um _wrapper_ para a adição de leite, outro para a adição de açúcar, outro para a adição de canela... Como uma boneca russa.

![structure](https://github.com/connorharu/Padroes-de-Projeto/assets/142368559/e2c8c1e9-e759-4672-976c-7cade7b1787f)

_Texto ALT: diagrama UML sobre o padrão de criação Decorator. Imagem retirada do site refactoring.guru_.

### Código de exemplo
```cpp
#include <iostream>
using namespace std;

// Classe base: Café
class Cafe {
    // atributos de café aqui
public:
    virtual string preparar() { // retorna uma string com o café preparado, com adicionais, se houverem
        return "Café";
    }
};

// wrapper de adição de leite
class AdicaoLeite : public Cafe { // herda o método preparar de Cafe
private:
    Cafe* cafe; // aponta para um objeto de tipo Cafe
public:
    AdicaoLeite(Cafe* cafe) { // construtor de AdicaoLeite, que recebe um ponteiro para um objeto Cafe
        this->cafe = cafe;
    } 

    string preparar() override {
        return cafe->preparar() + ", com leite"; // override no preparar para adição de leite
    }
};

// wrapper de adição de açúcar
class AdicaoAcucar : public Cafe { // herda o método preparar de Cafe
private:
    Cafe* cafe; // aponta para um objeto de tipo Cafe
public:
    AdicaoAcucar(Cafe* cafe) { // construtor de AdicaoAcucar, que recebe um ponteiro para um objeto Cafe
        this->cafe = cafe;
    }

    string preparar() override {
        return cafe->preparar() + ", com açúcar"; // override no preparar para adição de açúcar
    }
};

// wrapper: adição de canela
class AdicaoCanela : public Cafe { // herda o método preparar de Cafe
private:
    Cafe* cafe; // aponta para um objeto de tipo Cafe
public:
    AdicaoCanela(Cafe* cafe) { // construtor de AdicaoCanela, que recebe um ponteiro para um objeto Cafe
        this->cafe = cafe;
    }

    string preparar() override {
        return cafe->preparar() + ", com canela"; // override no preparar para adição de canela
    }
};

int main() {
    // café básico
    Cafe* cafe = new Cafe();
    cout << cafe->preparar() << endl; // saída: café

    // café com Leite
    Cafe* cafe_com_leite = new AdicaoLeite(cafe);
    cout << cafe_com_leite->preparar() << endl; // saída: café, com leite

    // café com leite e açúcar
    Cafe* cafe_com_leite_e_acucar = new AdicaoAcucar(cafe_com_leite);
    cout << cafe_com_leite_e_acucar->preparar() << endl; // saída: café, com leite e açúcar

    // café com leite, açúcar e canela
    Cafe* cafe_com_leite_e_acucar_e_canela = new AdicaoCanela(cafe_com_leite_e_acucar);
    cout << cafe_com_leite_e_acucar_e_canela->preparar() << endl; // saída: café, com leite, açúcar e canela

    delete cafe_com_leite_e_acucar_e_canela;
    delete cafe_com_leite_e_acucar;
    delete cafe_com_leite;
    delete cafe;

    return 0;
}
```
## 
