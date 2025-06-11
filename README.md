# **Padrão de Projeto Decorator (GoF - Estrutural)**

## **📌 Introdução**
O Decorator é um padrão de projeto estrutural que permite adicionar novos comportamentos a objetos **dinamicamente**, sem alterar sua implementação original. Ele segue o **Open/Closed Principle** do SOLID, permitindo extensão sem modificar a classe base.

## **📌 Wrappers Vs Decorators** Definição Google
Wrappers and decorators are design patterns that allow you to modify or extend the behavior of a function or object without changing its original implementation. Wrappers are general-purpose functions that wrap the original function, adding extra behavior before or after its execution. Decorators are a specific type of wrapper that takes a function as input and returns a new function that wraps the original, often using a special syntax (e.g., @ in Python). 

## **🔎 Problema**
Em um sistema de cafés, cada combinação de ingredientes (leite, chantilly) exigiria subclasses específicas (`CafeComLeite`, `CafeComChantilly`), levando a uma explosão de classes.

## **💡 Solução**
O Decorator resolve isso com wrappers que:
1. Implementam a mesma interface do objeto original
2. Encapsulam uma instância do objeto
3. Adicionam comportamentos dinamicamente

## **📂 Exemplo em Python**
```python
from abc import ABC, abstractmethod

class Cafe(ABC):
    @abstractmethod
    def get_descricao(self): pass
    
    @abstractmethod
    def get_custo(self): pass

class CafeSimples(Cafe):
    def get_descricao(self): return "Café puro"
    def get_custo(self): return 3.0

class DecoradorDeCafe(Cafe, ABC):
    def __init__(self, cafe: Cafe):
        self._cafe = cafe

class Leite(DecoradorDeCafe):
    def get_descricao(self): return f"{self._cafe.get_descricao()}, com leite"
    def get_custo(self): return self._cafe.get_custo() + 1.5

class Chantilly(DecoradorDeCafe):
    def get_descricao(self): return f"{self._cafe.get_descricao()}, com chantilly"
    def get_custo(self): return self._cafe.get_custo() + 2.0

# Uso
cafe = Chantilly(Leite(CafeSimples()))
print(f"{cafe.get_descricao()} | Custo: R${cafe.get_custo():.2f}")
# Saída: Café puro, com leite, com chantilly | Custo: R$6.50
```
# Diagrama do Padrão Decorator
```
        +---------------------+
        |       Cafe          |  <-- Interface/Classe Abstrata
        +---------------------+
        | + get_descricao()   |
        | + get_custo()       |
        +----------↑----------+
                   |
    +--------------+--------------+
    |                             |
+---------------------+     +---------------------+
|    CafeSimples      |     |  DecoradorDeCafe    |  <-- Classe Abstrata
+---------------------+     +---------------------+
| + get_descricao()   |     | - _cafe: Cafe       |
| + get_custo()       |     | + get_descricao()   |
+---------------------+     | + get_custo()       |
                           +----------↑----------+
                                     |
                     +-----------------------------------+
                     |               |                   |
             +----------------+ +----------------+ +----------------+
             |     Leite      | |   Chantilly    | |    Canela      |
             +----------------+ +----------------+ +----------------+
             | + get_descricao| | + get_descricao| | + get_descricao|
             | + get_custo()  | | + get_custo()  | | + get_custo()  |
             +----------------+ +----------------+ +----------------+
```

# 💼 Aplicações Reais
Java I/O: BufferedReader(FileReader)

Middlewares em frameworks web

Componentes GUI

# Vantagens
✔ Flexibilidade para adicionar/remover comportamentos
✔ Evita herança múltipla
✔ Princípio de responsabilidade única

# 📚 Referências
GoF - Design Patterns (1994) Erich Gamma

Refactoring Guru https://refactoring.guru

Python ABC module https://docs.python.org/3/library/abc.html
