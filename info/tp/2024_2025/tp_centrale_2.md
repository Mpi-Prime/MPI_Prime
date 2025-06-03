# TP Centrale MPI - Sujet 1
Corrigé par Olivier 
## La musique comme chaîne de caractères 

### Contexte 
### Description des fichiers fournis
### Recherche de motifs 
Soit $`P = \langle p_1, \ldots, p_m \rangle `$ une mélodie (où $`\forall i, p_i \in M`$). 
On dit que $`P`$ est un motif d'une partition $`X = \langle x_1, \ldots, x_n \rangle `$ si il existe $`i`$ tel que $`\forall k \in ⟦ 1, m ⟧, p_k = x_{i+k}`$. 
Dans cette section, on propose de chercher un motif donné dans une partition.

✨ **Question 1.** `recherche : Musique.partition -> Musique.partition -> int ` dans `rech_naive.ml` est une implémentation d'un algorithme de recherche de motif. Implémenter un jeu de tests pour cette fonction, et corriger d'éventuelles erreurs d'implémentation.


J'ai trouvé une seule erreur (cf. le commentaire dans le code) 

```ocaml
(* extrait du rech_naive.ml *)
let recherche motif part : int =
  let len_motif = Array.length motif in 
  let len_part = Array.length part in 
  let rec parcours ind_part =
    if (ind_part = len_part || len_part - ind_part < len_motif )then (* ATTENTIONNNN, ON NE LANCE PAS LA FONCTION NON PLUS SI ON SAIT QU'ON N'EST PAS DANS LES BORNES *)
      -1
    else if est_prefixe motif part ind_part then 
      ind_part
    else
      parcours (ind_part + 1)
  in
  parcours 0
```

 Pour les jeux de tests, je voulais juste m'assurer que ma correction d'erreur fonctionnait bien 

```ocaml

let () =
  let chaine_test = [| Do; Re ; Mi |] ;;

  let test1 = [|Re;Mi|] (* Presence en i = 1*)
  let test2 = [|Do;Mi;Re;Sol|] (* Teste si le cas de bordure est traité*)

  let ind1 = recherche test1 chaine_test ;;
  let ind2 = recherche test2 chaine_test;;
  Printf.printf "%d \n" (ind1);;

  Printf.printf "%d \n" (ind2)
```

⚠️⚠️ Pour compiler, j'ai quand même dû écrire ` open Musique ` et `open Rech_naive` au début du main… 

✨ **Question 2.** Décrire la fonction et discuter de sa complexité et des choix faits en terme de tests. Quel(s) algorithme(s) plus efficace(s) pourrait-on utiliser ?

💡 Question de cours… On est en $`\mathcal(O)(|t| \times |m|)`$ avec $`|t|`$ la taille du texte et $`|m|`$ la taille du motif.

📝 Dans les recherches de motif dans un texte, on parle très souvent des algos de Boyer-Moore et de Rabin-Karp (l'empreinte de Rabin, suivant la manière dont on l'optimise, peut s'avérer très efficace!)

### Plus longue sous-séquence commune 
✨ **Question 3.** Donner la complexité d'un tel algorithme 

Le nombre de sous-séquence d'un texte est de $`2^n`$ (on prend une lettre ou on la prend pas). Parcourir toutes les sous-séquences pour des calculs d'appartenance nous mettrait en $`\mathcal(O)(2^n + 2^m + 2^n \times 2^m)`$, ce qui est exponentiellement abusé…


✨ **Question 4.** On considère qu'on a un tableau à deux dimensions $`c[0 \ldots n ][0 \ldots m ]`$, tel que $`c[i][j]`$ est la longueur des PLSC entre $`X_i`$ et $`Y_j`$. Donner la formule permettant de calculer $`c[i][j]`$ et expliquer la stratégie. 


La classique formule de prog dyn, on utilise le théorème 1 pour conclure : 

$$
c[i][j] = \begin{cases} 1 + c[i-1][j-1] \text{ si } x_i = y_j \\
\max ( c[i][j-1], c[i-1][j] )
\end{cases}
$$

et donc j'imagine qu'on veut, au vu des questions suivantes, calculer tous les coeffs donc c'est une strat _bottom-up_ !


✨ **Question 5.** Implémenter une fonction de type `tabPLSC : Musique.partition -> Musique.partition -> int array array` qui crée, remplit et revoie ce tableau pour deux partitions données.
```ocaml
let tabPLSC (text1 : partition) (text2 : partition) : int array array = 

  (* Initialisation du résultat *)
  let n = Array.length text1 in 
  let m = Array.length text2 in 
  let res = Array.make_matrix (n+1) (m+1) (-1) in 

  (* Initialisation des coeff sur le bord *)
  for j = 0 to m do 
    res.(0).(j) <- 0;
  done;

  for i = 0 to n do 
    res.(i).(0) <- 0;
  done; 


  (* Calcul global des coefficients*)
  for i= 1 to n do 
    for j = 1 to m do 
        if text1.(i-1) = text2.(j-1) then 
          res.(i).(j) <- 1 + res.(i-1).(j-1)
        else
          res.(i).(j) <- max (res.(i).(j-1)) (res.(i-1).(j))
    done;
  done;
  (* retour du résultat *)
  res


```


✨ **Question 6.** Pour `mus5` et `mus6` : 15



✨ **Question 7.** Proposer un algorithme pour cela. 

Question très classique, on souhaite reconstruire la solution à partir de la matrice issue de notre procédé de programmation dynamique ! Pour cela, on part du dernier élément $`c[n][m]`$ et on remonte vers le premier élément $`c[0][0]`$ en utilisant la logique inverse de la construction : 

Pour $`i \in ⟦1, n⟧, j \in ⟦1, m⟧`$, 

- Si $`x_{i-1} = y_{j-1}`$ : alors ce caractère fait partie de la sous-séquence recherchée. On l'ajoute et on remonte diagonalement à $`c[i-1][j-1]`$
- Sinon :
   - Si $`c[i-1][j] > c[i][j-1]`$ : alors la meilleure solution vient de $`c[i-1][j]`$ donc on remonte vers le haut (une ligne au dessus)
   - Sinon : on se déplace d'une colonne vers la gauche car la meilleure solution vient de $`c[i][j-1]`$
 


✨ **Question 8.** Implémenter une fonction de prototype `trouvePLSC : Musique.partition -> Musique.partition -> Musique.note list` qui renvoie la PLSC de deux partitions.
```ocaml
let trouvePLSC (text1 : partition) (text2 : partition) : note list = 
  let c = tabPLSC text1 text2 in 
  let rec parcours_tab (acc : note list) (i : int) (j : int) = 
    if i = 0 || j = 0 then acc 
    else begin 
      let li,lj = text1.(i-1), text2.(j-1) in 
      if li = lj then 
        parcours_tab (li::acc) (i-1) (j-1) 
      else begin 
        if c.(i-1).(j) > c.(i).(j-1) then parcours_tab acc (i-1) j 
        else parcours_tab acc i (j-1)
      end
    end 
  in parcours_tab [] (Array.length text1) (Array.length text2)

```
✨ **Question 9.** Donner la PLSC pour `Musique.mus5` et `Musique.mus6`

On a `Re Re Mi Fa Re Do La Sol La Re Do La Re Sol Fa`


✨ **Question 10.** Proposer un encodage pour les notes en fonction de l'arbre de Huffman de la figure 3.

Comme dans le cours, quand on va à gauche on met un 0, et un 1 quand on va à droite : 
 - Fa : 00
 - La : 0100
 - Si : 0101
 - Do : 011
 - Sol : 100
 - Mi : 101
 - Re : 11


✨ **Question 11.** Implémenter une fonction qui compte le nombre d'occurences de chaque note dans une partition de prototype `compte_occ : Musique.partition -> (Musique.note * int) list`
```ocaml
let compte_occ (text : partition) : (note * int) list = 
  (*Initialisation de variables*)
  let len = Array.length text in 
  let vus = ref [] in 
  let res = ref [] in 


  for i = 0 to len -1 do  (* on parcourt le tableau *)
    let note_tmp = text.(i) in 
    if not (List.mem note_tmp !vus) then (* Si on a jamais vu cette lettre auparavant, i.e si on ne l'a pas compté *)
    begin 
      vus := note_tmp::(!vus) ; (* on l'ajoute aux notes vues *)

      (* On compte les occurences à partir de i car on n'avait pas rencontré cette note avant *)
      let count = ref 1 in 
      for j=i+1 to len-1 do 
        if text.(j) = note_tmp then incr(count)
      done;

      (* on modifie le résultat *)
      res := (note_tmp,!count)::(!res)
    end
  done;
  !res
```

✨ **Question 12.** Pour dérouler l'algorithme de Huffman, on a besoin d'une file de priorité. Discuter les implémentations possibles d'une telle structure 

Je vois trois possibilités (de la plus simple vers la plus complexe) : 

 - Liste triée (on trie la liste à chaque insertion) : l'insertion est en $`\mathcal{O} (n) `$ (on insère dans une liste triée) et l'extraction en $`\mathcal{O}(1)`$
 - Tas binaire implémenté avec un tableau : l'insertion et l'extraction sont en $`\mathcal{O}(\log(n))`$
 - Tas de Fibonacci : Insertion en $`\mathcal{O}(1)`$ et extraction en $`\mathcal{O}(\log(n))`$ pour la complexité amortie





✨ **Question 13.** Définir le type de votre file de priorité, avec comme priorité un entier, et comme valeur un élément du type `Arbre.arbre`, décrit dans `arbre.mli`. Implémenter les fonctions nécessaires à sa manipulation.

```ocaml
type liste_prio = (arbre * int) list  (* cette liste est censée être triée *)

exception Liste_vide 



let rec insert (l : liste_prio) (elt : arbre) (prio : int) : liste_prio = 
  match l with 
  |[] -> [(elt, prio)]
  |x::xs when snd(x) >= prio -> (elt,prio)::l 
  |x::xs -> x::(insert xs elt prio)


exception Not_extract;;

let extraire_elt (a : arbre) : note = 
  match a with 
  |Leaf(f) -> f
  |_ -> raise Not_extract


let extraire_min (l : liste_prio) : arbre * liste_prio = 
  match l with 
  |[] -> raise Liste_vide
  |x::xs -> fst(x), xs
```


