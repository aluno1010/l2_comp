retirar comentarios



# A. Pontuação Máxima de Palavras Formadas por Letras(python)

def solve():
    n = int(input().strip())
    words = input().strip().split()
    letters_str = input().strip()
    scores = list(map(int, input().strip().split()))
    
    avail_counts = [0] * 26
    for char in letters_str:
        avail_counts[ord(char) - ord('a')] += 1
        
    valid_words = []
    for w in words:
        req = [0] * 26
        possible = True
        word_score = 0
        
        for char in w:
            idx = ord(char) - ord('a')
            req[idx] += 1
            word_score += scores[idx]
            
            if req[idx] > avail_counts[idx]:
                possible = False
                
        if possible:
            valid_words.append((req, word_score, w))

    m = len(valid_words)

    suffix_max = [0] * (m + 1)
    for i in range(m - 1, -1, -1):
        suffix_max[i] = suffix_max[i+1] + valid_words[i][1]

    max_score_found = 0

    def branch_and_bound(word_index, current_score, current_avail):
        nonlocal max_score_found

        if current_score > max_score_found:
            max_score_found = current_score

        if word_index == m:
            return

        if current_score + suffix_max[word_index] <= max_score_found:
            return

        branch_and_bound(word_index + 1, current_score, current_avail)

        req, w_score, _ = valid_words[word_index]
        can_include = True
        for i in range(26):
            if current_avail[i] < req[i]:
                can_include = False
                break

        if can_include:
            for i in range(26):
                current_avail[i] -= req[i]

            branch_and_bound(word_index + 1, current_score + w_score, current_avail)

            for i in range(26):
                current_avail[i] += req[i]

    branch_and_bound(0, 0, avail_counts)

    print(max_score_found)

if __name__ == '__main__':
    solve()

# A. Pontuação Máxima de Palavras Formadas por Letras (JAVA)

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {
    
    static int maxScoreFound = 0;
    static int[] suffixMax;
    static List<Word> validWords = new ArrayList<>();

    static class Word {
        int[] req = new int[26];
        int score = 0;
        String str;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (!sc.hasNextInt()) {
            sc.close();
            return;
        }

        int n = sc.nextInt();
        String[] words = new String[n];
        for (int i = 0; i < n; i++) {
            words[i] = sc.next();
        }

        String lettersStr = sc.next();

        int[] scores = new int[26];
        for (int i = 0; i < 26; i++) {
            scores[i] = sc.nextInt();
        }

        int[] availCounts = new int[26];
        for (char c : lettersStr.toCharArray()) {
            availCounts[c - 'a']++;
        }

        for (String w : words) {
            Word wordObj = new Word();
            wordObj.str = w;
            boolean possible = true;

            for (char c : w.toCharArray()) {
                int idx = c - 'a';
                wordObj.req[idx]++;
                wordObj.score += scores[idx];

                if (wordObj.req[idx] > availCounts[idx]) {
                    possible = false;
                }
            }

            if (possible) {
                validWords.add(wordObj);
            }
        }

        int m = validWords.size();

        suffixMax = new int[m + 1];
        for (int i = m - 1; i >= 0; i--) {
            suffixMax[i] = suffixMax[i + 1] + validWords.get(i).score;
        }

        branchAndBound(0, 0, availCounts);


        System.out.println(maxScoreFound);
        sc.close();
    }

    static void branchAndBound(int wordIndex, int currentScore, int[] currentAvail) {

        if (currentScore > maxScoreFound) {
            maxScoreFound = currentScore;
        }

        if (wordIndex == validWords.size()) {
            return;
        }


        if (currentScore + suffixMax[wordIndex] <= maxScoreFound) {
            return;
        }


        branchAndBound(wordIndex + 1, currentScore, currentAvail);

        Word w = validWords.get(wordIndex);
        boolean canInclude = true;
        for (int i = 0; i < 26; i++) {
            if (currentAvail[i] < w.req[i]) {
                canInclude = false;
                break;
            }
        }

        if (canInclude) {
            // Consome as letras (Backtracking step 1)
            for (int i = 0; i < 26; i++) {
                currentAvail[i] -= w.req[i];
            }

            branchAndBound(wordIndex + 1, currentScore + w.score, currentAvail);

            for (int i = 0; i < 26; i++) {
                currentAvail[i] += w.req[i];
            }
        }
    }
}

B. Combinação de Soma (python)

def solve():
    try:
        # Lê a primeira linha e extrai 'n' e 'alvo'
        primeira_linha = input().strip().split()
        if not primeira_linha:
            return
        
        n = int(primeira_linha[0])
        target = int(primeira_linha[1])
        
        # Lê a segunda linha e transforma em uma lista de inteiros
        segunda_linha = input().strip().split()
        candidates = [int(x) for x in segunda_linha]
        
        # Passo crucial: Ordenar para permitir a poda (Bound) e evitar duplicatas
        candidates.sort()
        results = []
        
        def branch_and_bound(start_index, current_comb, current_sum):
            # Caso base: Atingimos exatamente o alvo
            if current_sum == target:
                results.append(list(current_comb))
                return
            
            # Ramificação (Branch)
            for i in range(start_index, n):
                # Poda (Bound): Se a soma exceder o alvo, paramos de explorar.
                # Como a lista está ordenada, os próximos também excederão.
                if current_sum + candidates[i] > target:
                    break
                
                # Evita combinações duplicadas ignorando números iguais na mesma profundidade
                if i > start_index and candidates[i] == candidates[i - 1]:
                    continue
                
                # Escolhe o candidato atual e avança
                current_comb.append(candidates[i])
                branch_and_bound(i + 1, current_comb, current_sum + candidates[i])
                
                # Retrocede (Backtrack) para testar a próxima possibilidade
                current_comb.pop()
                
        # Inicia a busca a partir do índice 0, com combinação vazia e soma 0
        branch_and_bound(0, [], 0)
        
        # Imprime os resultados formatados
        for res in results:
            print(" ".join(map(str, res)))

    except EOFError:
        # Trata o caso onde o fim do arquivo é atingido inesperadamente
        pass

if __name__ == '__main__':
    solve()

 B. Combinação de Soma (java)   

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;

public class CombinationSum {
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // Verifica se há entrada disponível
        if (!scanner.hasNextInt()) {
            scanner.close();
            return;
        }
        
        int n = scanner.nextInt();
        int target = scanner.nextInt();
        
        int[] candidates = new int[n];
        for (int i = 0; i < n; i++) {
            candidates[i] = scanner.nextInt();
        }
        
        // Passo crucial: Ordenar a matriz
        Arrays.sort(candidates);
        List<List<Integer>> results = new ArrayList<>();
        
        // Inicia o processo de Branch and Bound
        branchAndBound(candidates, target, 0, new ArrayList<>(), 0, results);
        
        // Imprime os resultados
        for (List<Integer> res : results) {
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < res.size(); i++) {
                sb.append(res.get(i));
                if (i < res.size() - 1) {
                    sb.append(" ");
                }
            }
            System.out.println(sb.toString());
        }
        
        scanner.close();
    }
    
    private static void branchAndBound(int[] candidates, int target, int startIndex, 
                                       List<Integer> currentComb, int currentSum, 
                                       List<List<Integer>> results) {
        // Caso base: soma igual ao alvo
        if (currentSum == target) {
            results.add(new ArrayList<>(currentComb));
            return;
        }
        
        // Ramificação (Branch)
        for (int i = startIndex; i < candidates.length; i++) {
            
            // Poda (Bound): Interrompe o loop se a soma atual + candidato ultrapassar o alvo
            if (currentSum + candidates[i] > target) {
                break;
            }
            
            // Evita criar ramos idênticos (Combinações duplicadas)
            if (i > startIndex && candidates[i] == candidates[i - 1]) {
                continue;
            }
            
            // Adiciona candidato e busca recursivamente o próximo elemento (i + 1)
            currentComb.add(candidates[i]);
            branchAndBound(candidates, target, i + 1, currentComb, currentSum + candidates[i], results);
            
            // Backtrack: Remove o último candidato inserido para tentar a próxima possibilidade
            currentComb.remove(currentComb.size() - 1);
        }
    }
}

# C. dragoes em python

def resolver_dragoes():
    # Lê a força inicial s e a quantidade de dragões n
    entrada = input().split()
    if not entrada:
        return
        
    s = int(entrada[0])
    n = int(entrada[1])
    
    dragoes = []
    
    # Lê as informações de cada dragão
    for _ in range(n):
        x, y = map(int, input().split())
        dragoes.append((x, y))
        
    # Ordena os dragões pela sua força (o primeiro elemento da tupla 'x')
    dragoes.sort(key=lambda dragao: dragao[0])
    
    # Aplica a estratégia gulosa
    for x, y in dragoes:
        if s > x:
            s += y # Kirito vence e ganha o bônus
        else:
            print("NO") # Kirito não é forte o suficiente
            return
            
    # Se passou por todos os dragões, ele venceu
    print("YES")

if __name__ == "__main__":
    resolver_dragoes()


# c dragores em java

import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;

public class Main {
    
    // Classe para representar um dragão
    static class Dragao {
        int forca;
        int bonus;
        
        public Dragao(int forca, int bonus) {
            this.forca = forca;
            this.bonus = bonus;
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // Verifica se há entrada para evitar erros
        if (!scanner.hasNextInt()) {
            scanner.close();
            return;
        }

        int s = scanner.nextInt();
        int n = scanner.nextInt();
        
        Dragao[] dragoes = new Dragao[n];
        
        // Lê as informações de cada dragão
        for (int i = 0; i < n; i++) {
            dragoes[i] = new Dragao(scanner.nextInt(), scanner.nextInt());
        }
        
        // Ordena o array de dragões pela força (crescente)
        Arrays.sort(dragoes, Comparator.comparingInt(d -> d.forca));
        
        boolean venceuTodos = true;
        
        // Aplica a estratégia gulosa
        for (Dragao d : dragoes) {
            if (s > d.forca) {
                s += d.bonus; // Kirito vence e ganha o bônus
            } else {
                venceuTodos = false; // Kirito perde
                break;
            }
        }
        
        // Imprime o resultado final
        if (venceuTodos) {
            System.out.println("YES");
        } else {
            System.out.println("NO");
        }
        
        scanner.close();
    }
}
