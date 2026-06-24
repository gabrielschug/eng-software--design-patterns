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



