import itertools, collections

class Fronteira:
    def __init__(self, jogo, pai=None, movimento='Estado Inicial'):
        self.jogo = jogo
        self.pai = pai
        self.movimento = movimento
        if (self.pai != None):
            self.g = pai.g + 1
        else:
            self.g = 0

    @property
    def state(self):
        return str(self)

    @property 
    def path(self):
        fronteira, p = self, []
        while fronteira:
            p.append(fronteira)
            fronteira = fronteira.pai
        yield from reversed(p)

    @property
    def pronto(self):
        return self.jogo.pronto

    @property
    def movimentos(self):
        return self.jogo.movimentos

    @property
    def h(self):
        return self.jogo.manhattan

    @property
    def f(self):
        return self.h + self.g

    def __str__(self):
        return str(self.jogo)

class ResolveJogo:
    def __init__(self, noInicial):
        self.noInicial = noInicial

    def busca(self):
        lista = collections.deque([Fronteira(self.noInicial)])
        explorados = set()
        explorados.add(lista[0].state)
        while lista:
            lista = collections.deque(sorted(list(lista), key=lambda fronteira: fronteira.f))
            fronteira = lista.popleft()
            if fronteira.pronto:
                return fronteira.path

            for mover, movimento in fronteira.movimentos:
                sucessor = Fronteira(mover(), fronteira, movimento)
                if sucessor.state not in explorados:
                    lista.appendleft(sucessor)
                    explorados.add(sucessor.state)

class Jogo:
    def __init__(self, tabuleiro):
        self.width = len(tabuleiro[0])
        self.tabuleiro = tabuleiro

    @property
    def pronto(self):
       
        N = self.width * self.width
        return str(self) == ''.join(map(str, range(1,N))) + '0'

    @property 
    def movimentos(self):
        def criaJogada(at, to):
            return lambda: self._jogada(at, to)
        jogadas = []
        for i, j in itertools.product(range(self.width),range(self.width)):
            paraOnde = {'Mova o 0 para Direita':(i, j-1),
                      'Mova o 0 para Esquerda':(i, j+1),
                      'Mova o 0 para Baixo':(i-1, j),
                      'Mova o 0 para Cima':(i+1, j)}
            for movimento, (r, c) in paraOnde.items():
                if r >= 0 and c >= 0 and r < self.width and c < self.width and \
                   self.tabuleiro[r][c] == 0:
                    mover = criaJogada((i,j), (r,c)), movimento
                    jogadas.append(mover)
        return jogadas

    @property
    def manhattan(self):
        distanciaNo = 0
        for i in range(3):
            for j in range(3):
                if self.tabuleiro[i][j] != 0:
                    x, y = divmod(self.tabuleiro[i][j]-1, 3)
                    distanciaNo += abs(x - i) + abs(y - j)
        return distanciaNo

    def copy(self):
        tabuleiro = []
        for row in self.tabuleiro:
            tabuleiro.append([x for x in row])
        return Jogo(tabuleiro)

    def _jogada(self, at, to):
        copy = self.copy()
        i, j = at
        r, c = to
        copy.tabuleiro[i][j], copy.tabuleiro[r][c] = copy.tabuleiro[r][c], copy.tabuleiro[i][j]
        return copy

    def pprint(self):
        for row in self.tabuleiro:
            print(row)
        print()

    def __str__(self):
        return ''.join(map(str, self))

    def __iter__(self):
        for row in self.tabuleiro:
            yield from row




#COLOCAR AQUI AS POSIÇÕES   
tabuleiro = [[8,6,4],[5,0,3],[2,7,1]]



jogo = Jogo(tabuleiro)


s = ResolveJogo(jogo)
p = s.busca()


total_movimentos = 0
for fronteira in p:
    print(fronteira.movimento)
    fronteira.jogo.pprint()
    total_movimentos += 1

print("Solução Ótima em : " + str(total_movimentos) +" Movimentos")
