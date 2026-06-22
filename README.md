--Taxi-- Gulosos

n = int(input())
grupos = list(map(int, input().split()))

qtd = [0] * 5

for g in grupos:
    qtd[g] += 1

taxis = qtd[4]

# Grupos de 3
taxis += qtd[3]
qtd[1] = max(0, qtd[1] - qtd[3])

# Grupos de 2
taxis += qtd[2] // 2

if qtd[2] % 2:
    taxis += 1
    qtd[1] = max(0, qtd[1] - 2)

# Grupos de 1 restantes
taxis += (qtd[1] + 3) // 4

print(taxis)





-- Menor Intevalo - gulosos 
n, k = map(int, input().split())
nums = list(map(int, input().split()))

maior = max(nums)
menor = min(nums)

resposta = maior - menor - 2 * k

print(max(0, resposta))




-- branch and bound -- cookies
n, k = map(int, input().split())
cookies = list(map(int, input().split()))

cookies.sort(reverse=True)

melhor = float('inf')
criancas = [0] * k

def bt(i):
    global melhor

    if i == n:
        melhor = min(melhor, max(criancas))
        return

    if max(criancas) >= melhor:
        return

    for j in range(k):
        criancas[j] += cookies[i]

        bt(i + 1)

        criancas[j] -= cookies[i]

        # poda de simetria
        if criancas[j] == 0:
            break

bt(0)

print(melhor)




-- branch and bound -- Matchsticks to Square
n = int(input())
matchsticks = list(map(int, input().split()))

total = sum(matchsticks)

if total % 4 != 0:
    print("false")
    exit()

lado = total // 4

matchsticks.sort(reverse=True)

if matchsticks[0] > lado:
    print("false")
    exit()

lados = [0] * 4

def bt(i):
    if i == n:
        return lados[0] == lados[1] == lados[2] == lados[3] == lado

    for j in range(4):

        if lados[j] + matchsticks[i] > lado:
            continue

        lados[j] += matchsticks[i]

        if bt(i + 1):
            return True

        lados[j] -= matchsticks[i]

        # poda de simetria
        if lados[j] == 0:
            break

    return False

print(str(bt(0)).lower())
