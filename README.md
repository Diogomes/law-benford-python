# law-benford-python
basic law enforcement using python

1. Lei de benford
função que recebe uma string e imprime a proporcão que cada um dos digitos (1 a 9) aparece no texto como o primeiro digito de um numero - essa é a verdadeira lei de Benford - nao a que implementamos em haskell.

2. OO em Python

class C:
  def __init__(self,...)

  def __repl__(self)

  def __str__(self)

  def __eq__(self, other)   self == other?

  def __add__(self,other)   self + other

Python data model...

3. Iterators

O que pode estar no "for"

    for x in z:

implementam o metodo =__next__= que é chamado pela funcao =next= que levanta a excecao =StopIteration= quando nao há proximo.

try:
   it = iter(z)
   while True:
     x = next(it)
     ...
except StopIteration:
   pass

class meurange:
  def __init__(self,baixo,alto):
      self.baixo=baixo
      self.alto=alto
      self.i = baixo-1
  def __iter__(self):
      return self
  def __next__(self):
      if self.i<self.alto:
         self.i = self.i+1
         return self.i
      else: 
         raise StopIteration

alguns iterators sÃo como os thunk de haskell - promessas de computacão.

range(1,10)

map(lambda x: x+1,[1,2,3])

mas imprimir na tela nao força a execução das promessas. =list= executa as promessas e cria uma lista.

range(1,20)

print(range(1,20))

list(range(1,20))

4. Generators
Funcoes que guardam o estado entre uma chamada e outra e fazem o papel de um iterator

Use =yield= em vez de =return= - na proxima chamada a funçao executa do =yield= ate o proximo =yield=.

def meurange(baixo,alto):
   baixo=baixo-1
   while baixo< alto:
      baixo = baixo+1
      yield baixo

5. Decorators
Funcoes que recebem uma funcao e retornam outra funçao que "envolve" aquela:

import time

def tempo(func):
  def aux():
      t1=time.time()
      for i in range(1000):
         func()
      t2=time.time()
      print(t2-t1)
  return aux
def a():
   y=[]
   for x in range(10):
       y.append(x)

def b():
  y=[0]*10
  for x in range(10):
    y[x]=x


a()

a=tempo(a)
b=tempo(b)

a()
b()

decorators sao essas funcoes + syntatic sugar.

@tempo
def a():
   y=[]
   for x in range(10):
       y.append(x)

@tempo
def b():
  y=[0]*10
  for x in range(10):
    y[x]=x

Como fazer decorators para funcoes com argumentos:

def trac(funcao):
   def aux(*args):
     print("inicio args:",args)
     x = funcao(*args)
     print("fim resultado :",x)
     return x
   return aux

@trac
def f(a,b,c):
   return a*b-c

memorizador

def memoriza(f):
   mem={}
   def aux(*args): 
     if args in mem:
        return mem[args]
     else:
        x = f(*args)
        mem[args] = x
        return x
   return aux
   