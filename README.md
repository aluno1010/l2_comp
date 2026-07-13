

Q1 — Maior Substring Exemplar


def maior_substring_exemplar(s):
    if not s:
        return ""

    contidas = set(s)

    for c in s:
        if c.isalpha() and c.swapcase() not in contidas:
            partes = s.split(c)
            melhor = ""
            for parte in partes:
                candidato = maior_substring_exemplar(parte)
                if len(candidato) > len(melhor):
                    melhor = candidato
            return melhor

    return s


def main():
    s = input()
    resultado = maior_substring_exemplar(s)
    print(resultado if resultado != "" else "0")


if __name__ == '__main__':
    main()

Complexidade: O(n²) no pior caso.

---


Q2 — Investimento Certeiro

def maior_lucro(precos):
    n = len(precos)
    if n == 0:
        return 0

    dp = [0] * n  # dp[i] = maior lucro possivel considerando os dias ate i
    menor_preco = precos[0]

    for i in range(1, n):
        dp[i] = max(dp[i - 1], precos[i] - menor_preco)
        menor_preco = min(menor_preco, precos[i])

    return dp[n - 1]


def main():
    precos = list(map(int, input().split()))
    print(maior_lucro(precos))


if __name__ == '__main__':
    main()

Complexidade: O(n) tempo, O(n) espaço.

---
