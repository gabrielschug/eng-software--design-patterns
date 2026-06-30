# eng-software--design-patterns
## Singleton

### O que é?
O Singleton é um padrão de projeto criacional que garante que uma classe possua apenas uma única instância durante toda a execução do sistema, fornecendo um ponto global de acesso a essa instância.
Esse padrão é utilizado quando diversos componentes precisam compartilhar o mesmo objeto, evitando a criação de múltiplas instâncias que poderiam causar inconsistências ou desperdício de recursos.


## Quais problemas ele resolve?
Imagine um sistema de gerenciamento de uma empresa que possui uma configuração global do sistema.
Sem o padrão Singleton, cada módulo poderia criar sua própria instância das configurações, resultando em:

Consumo desnecessário de memória;
Informações inconsistentes entre módulos;
Dificuldade de sincronização dos dados;
Maior complexidade de manutenção.

O Singleton resolve esse problema garantindo que todos os módulos utilizem exatamente a mesma instância da configuração.

## Como ele atua
A classe mantém uma referência privada para sua única instância.
Quando alguém solicita uma instância da classe, ela verifica se já existe.
Caso não exista, a instância é criada.
Caso já exista, a mesma instância é retornada.
Todas as partes do sistema compartilham o mesmo objeto.
 
## Pontos Fortes
Controle de Instância
Garante que exista apenas um objeto da classe durante toda a execução do sistema.

Economia de Recursos
Evita a criação desnecessária de múltiplos objetos, reduzindo consumo de memória e processamento.

Acesso Global
Permite que qualquer parte do sistema utilize a mesma instância facilmente.

Consistência dos Dados
Todos os módulos trabalham sobre o mesmo conjunto de informações.




## Pontos Fracos
Dificulta Testes
Como existe apenas uma instância global, pode ser complicado criar testes independentes.

Alto Acoplamento
Outras classes podem se tornar dependentes diretamente do Singleton.

Problemas em Sistemas Concorrentes
Em aplicações com múltiplas threads, é necessário implementar mecanismos adicionais para evitar a criação simultânea de mais de uma instância.

Viola o Princípio da Responsabilidade Única
Além de sua função principal, a classe passa a controlar seu próprio ciclo de vida.

Pode Ser Utilizado em Excesso
Muitos desenvolvedores utilizam Singleton quando uma simples injeção de dependência resolveria o problema de forma mais adequada.

## Com o Padrão
```python
# singleton/com_padrao/configuracao.py

class ConfiguracaoSistema:

    _instancia = None

    def __new__(cls):

        if cls._instancia is None:
            print("Criando instância única...")
            cls._instancia = super().__new__(cls)

        return cls._instancia

    def __init__(self):
        self.tema = "Escuro"
        self.idioma = "Português"


config1 = ConfiguracaoSistema()
config2 = ConfiguracaoSistema()
config3 = ConfiguracaoSistema()

print(config1 == config2)
print(config2 == config3)
```

## Sem o Padrão
```python
# singleton/sem_padrao/configuracao.py

class ConfiguracaoSistema:

    def __init__(self):
        print("Carregando configurações do sistema...")
        self.tema = "Escuro"
        self.idioma = "Português"


# Cada setor cria sua própria configuração

config1 = ConfiguracaoSistema()
config2 = ConfiguracaoSistema()
config3 = ConfiguracaoSistema()

print(config1 == config2)
```

## Prototype

### O que é?
O Prototype é um padrão de projeto criacional que permite copiar objetos existentes sem fazer seu código ficar dependente de suas classes 

## Quais problemas ele resolve
O padrão Prototype resolve o problema de criar objetos complexos ou semelhantes repetidamente. Em vez de construir cada objeto do zero, ele permite fazer uma cópia de um objeto já existente, economizando tempo, reduzindo a repetição de código e facilitando a criação de várias instâncias com as mesmas características

## Como ele atua
Na prática, o padrão Prototype funciona criando um objeto base já configurado e depois gerando novos objetos a partir dele usando clonagem. Assim, em vez de usar new e configurar tudo de novo, você apenas chama um método como clone(), que cria uma cópia do objeto original, podendo depois ajustar alguns detalhes se necessário. 

## Pontos positivos e negativos
Pontos Positivos:
 Mais rápido para criar objetos complexos.
 Evita repetir código de inicialização.
 Facilita a criação de muitos objetos semelhantes.
 Reduz o acoplamento com classes concretas.

Pontos Negativos:
 Implementar a clonagem pode ser complexo.
 Objetos com referências a outros objetos podem gerar problemas de cópia.
 Alteração na estrutura da classe podem exigir ajustes no método de clonagem.
 Pode aumentar a dificuldade de manutenção se houver muitas regras de clonagem.

## Codigo sem o padrão
```python
class Personagem:
    def __init__(self, nome, classe, inventario):
        self.nome = nome
        self.classe = classe
        self.inventario = inventario

    def __str__(self):
        return f"{self.nome} [{self.classe}] - Inventário: {self.inventario}"

guerreiro_base = Personagem("Soldado", "Guerreiro", ["Espada de Madeira", "Escudo"])

heroi = Personagem(
    nome=guerreiro_base.nome, 
    classe=guerreiro_base.classe, 
    inventario=list(guerreiro_base.inventario) 
)

heroi.nome = "Arthur"
heroi.inventario.append("Poção de Vida")

print("Objeto Original:", guerreiro_base)
print("Novo Objeto:    ", heroi)
```
## Codigo com o padrão
```python
class Prototype:
    def clone(self):
        return copy.deepcopy(self)

class Personagem(Prototype):
    def __init__(self, nome, classe, inventario):
        self.nome = nome
        self.classe = classe
        self.inventario = inventario # Uma lista (objeto mutável)

    def __str__(self):
        return f"{self.nome} [{self.classe}] - Inventário: {self.inventario}"

guerreiro_base = Personagem("Soldado", "Guerreiro", ["Espada de Madeira", "Escudo"])

heroi = guerreiro_base.clone()

heroi.nome = "Arthur"
heroi.inventario.append("Poção de Vida")

print("Protótipo Original:", guerreiro_base)
print("Clone Modificado:  ", heroi)
```
# 3. 🏭 Fabric Method (Método de Fábrica)

> Aluno: Gabriel Schug

## 🔄 Sem o Padrão

- o código principal do aplicativo cria objetos de forma direta usando o comando `new`.
  - gera um alto acoplamento
  - obriga o sistema a usar estruturas condicionais (`if/else`) para adivinhar qual classe instanciar

```
// 1. PRODUTO (A Interface)
interface AtividadeFisica {
  iniciarTracker(): void;
}

// 2. PRODUTOS CONCRETOS
class Corrida implements AtividadeFisica {
  iniciarTracker(): void {
    console.log("Iniciando GPS para Corrida...");
  }
}

class Pedalada implements AtividadeFisica {
  iniciarTracker(): void {
    console.log("Iniciando GPS para Pedalada...");
  }
}

// 3. CRIADOR (O Gerenciador Base)
class TreinoApp {
  iniciarAtividade(tipoAtividade: string): void {
    let atividade: AtividadeFisica | null = null;

    if (tipoAtividade === "corrida") {
      atividade = new Corrida();       // 4. CRIADOR CONCRETO (inline, ainda sem fábrica)
    } else if (tipoAtividade === "pedalada") {
      atividade = new Pedalada();      // 4. CRIADOR CONCRETO (inline, ainda sem fábrica)
    } else {
      console.log("Atividade não suportada!");
      return;
    }

    atividade.iniciarTracker();
  }
}
```

O Problema:

- Ao adicionar um novo esporte, o programador abre a classe **`TreinoApp` e adiciona mais um `else if`.
- Viola regras de um bom design, pois o código principal precisa ser modificado constantemente e corre o risco de quebrar o que já funciona

### ⁉️ O que é?

É um **padrão de projeto criacional**.

Ele define uma interface (um molde) para a criação de um objeto, mas **delega às subclasses a decisão de qual classe exata será instanciada**.

Ao invés do sistema principal criar o objeto de forma direta e engessada, o sistema delega esta tarefa para uma "fábrica" especializada, permitindo que o programa se mantenha independente de como seus objetos são realmente construídos.

### 💡 Quais problemas ele resolve?

#### **O perigo do acoplamento forte gerado pelo comando ```new```**

**Quando você cria um objeto instanciando a classe diretamente no seu código principal (ex: `Atividade a = new Corrida()`), o seu programa fica rigidamente amarrado àquela classe específica**.

Se no futuro precisar suportar ```Atividade a = new Pedalada()```, será obrigado a caçar todas as ocorrências do comando **`new` espalhadas pelo sistema e adicionar blocos gigantes de verificações condicionais (`if/else`ou`switch`) para decidir qual classe instanciar**.

**Resultado** → código sujo → difícil de manter →  alto risco de quebrar o que já estava funcionando perfeitamente

## ⚙️ Como ele atua

Substitui as chamadas diretas do construtor (```new```) por chamadas a um **método-fábrica especial**.

Os objetos ainda são criados usando o `new`, mas de forma isolada, **dentro do método-fábrica**.

**Ele se organiza através de 4 participantes principais**:

* **Produto (A Interface):** Uma regra comum que todos os objetos criados devem seguir.
* **Produtos Concretos:** As implementações reais dessa interface (as classes específicas).
* **Criador (O Gerenciador Base):** Uma classe que possui a lógica central do sistema e declara o método-fábrica abstrato que devolverá um Produto.
* **Criadores Concretos (As Fábricas):** Subclasses que sobrescrevem o método-fábrica para devolver um Produto Concreto específico.


## Com o Padrão

```
// 1. PRODUTO (A Interface Comum)
interface AtividadeFisica {
  iniciarTracker(): void;
}

// 2. PRODUTOS CONCRETOS
class Corrida implements AtividadeFisica {
  iniciarTracker(): void {
    console.log("Iniciando GPS para Corrida...");
  }
}

class Pedalada implements AtividadeFisica {
  iniciarTracker(): void {
    console.log("Iniciando GPS para Pedalada...");
  }
}

// 3. CRIADOR (A Classe Base com o Factory Method)
abstract class GerenciadorDeTreino {

  // O Factory Method: abstrato, sem implementação aqui.
  abstract criarAtividade(): AtividadeFisica;

  // Lógica principal: usa o Produto fabricado sem saber sua classe exata.
  processarTreino(): void {
    const atividade = this.criarAtividade(); // Chama a fábrica
    atividade.iniciarTracker();              // Usa o produto fabricado
    console.log("Treino salvo no banco de dados com sucesso!");
  }
}

// 4. CRIADORES CONCRETOS (As Fábricas Específicas)
class GerenciadorCorrida extends GerenciadorDeTreino {
  criarAtividade(): AtividadeFisica {
    return new Corrida(); // Só esta fábrica sabe instanciar uma Corrida
  }
}

class GerenciadorPedalada extends GerenciadorDeTreino {
  criarAtividade(): AtividadeFisica {
    return new Pedalada(); // Só esta fábrica sabe instanciar uma Pedalada
  }
}

// CÓDIGO CLIENTE
const gerenciador: GerenciadorDeTreino = new GerenciadorCorrida();
gerenciador.processarTreino();
```

A solução:

- O código do cliente (o **`TreinoApp` e o `GerenciadorDeTreino`) não possui mais nenhum bloco `if/else` fazendo verificações
- Trabalha com qualquer classe apenas através da interface `AtividadeFisica`.
- Se futuramente adicionar ****Natação****, não será necessário alterar nenhuma linha do que já está pronto.
- Apenas criar dois arquivos novos no projeto: 1. classe `Natacao` 2. classe `GerenciadorNatacao`.
- 🔀 Flexibilidade 🚀 Escala


## 💪 Pontos Fortes

* **Elimina o acoplamento forte:** O código principal lida apenas com interfaces, ficando isolado das classes de implementação específicas**.**
* Permite adicionar novos produtos (novas atividades físicas) de forma extremamente fácil, sem quebrar o código já existente**.**
* Move a criação de objetos para um único local (fábricas), facilitando a manutenção geral do software

## ⚠️ Pontos Fracos

**Ele pode exigir a introdução de muitas subclasses novas no sistema**. Isso acontece porque, para cada novo produto (modalidade de exercício) criado, o desenvolvedor pode ser forçado a criar uma subclasse de criador (fábrica) correspondente apenas para instanciá-lo**. Isso faz a quantidade de arquivos e classes do projeto crescer rapidamente.





