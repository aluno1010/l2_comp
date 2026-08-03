
laranjas apodrecendo codigo abaixo:

def main():
    m, n = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(m)]

    fila = []
    frescas = 0
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 2:
                fila.append((i, j))
            elif grid[i][j] == 1:
                frescas += 1

    if frescas == 0:
        print(0)
        return

    minutos = 0
    direcoes = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    while fila and frescas > 0:
        proxima = []
        for x, y in fila:
            for dx, dy in direcoes:
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] == 1:
                    grid[nx][ny] = 2
                    frescas -= 1
                    proxima.append((nx, ny))
        if proxima:
            minutos += 1
        fila = proxima

    print(minutos if frescas == 0 else -1)


if __name__ == '__main__':
    main()



-----------------------------------------
codigo 2: cronograma de aulas codigo abaixo: 

def main():
    n, m = map(int, input().split())
    adjacencia = [[] for _ in range(n)]
    grau_entrada = [0] * n

    for _ in range(m):
        a, b = map(int, input().split())
        adjacencia[b].append(a)
        grau_entrada[a] += 1

    pilha = [i for i in range(n) if grau_entrada[i] == 0]
    ordem = []

    while pilha:
        atual = pilha.pop()
        ordem.append(atual)
        for prox in adjacencia[atual]:
            grau_entrada[prox] -= 1
            if grau_entrada[prox] == 0:
                pilha.append(prox)

    if len(ordem) == n:
        print(' '.join(map(str, ordem)))
    else:
        print('Impossivel')


if __name__ == '__main__':
    main()
----------------------------------------------------------------------------------------------------------------------------------------------------




codigos melhorados: cronograma de aula

def main():
    n, m = map(int, input().split())
    adjacencia = [[] for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        adjacencia[b].append(a)

    estado = [0] * n  # 0 = nao visitado, 1 = em andamento, 2 = concluido
    pos = [0] * n
    postorder = []

    for inicio in range(n):
        if estado[inicio] != 0:
            continue
        pilha = [inicio]
        estado[inicio] = 1
        while pilha:
            atual = pilha[-1]
            if pos[atual] < len(adjacencia[atual]):
                prox = adjacencia[atual][pos[atual]]
                pos[atual] += 1
                if estado[prox] == 1:
                    print('Impossivel')
                    return
                if estado[prox] == 0:
                    estado[prox] = 1
                    pilha.append(prox)
            else:
                estado[atual] = 2
                postorder.append(atual)
                pilha.pop()

    print(' '.join(map(str, reversed(postorder))))


if __name__ == '__main__':
    main()

 -------------------------------------------------
